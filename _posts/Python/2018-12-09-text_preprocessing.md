---
layout: post
title:  "영어 텍스트 전처리하기"
date:   2018-12-09 21:00:00
author: entiff
categories: Python
---

오늘은 텍스트 전처리를 이야기 해보고자 합니다. 텍스트 자체는 컴퓨터가 이해할 수 있는 방식이 아니기 때문에 모델에 넣고 연산할 수 있도록 몇가지 작업을 해야 합니다. 최근 참여하고 있는 Kaggle challenge인 Quora 텍스트 자료를 이용해 소개하겠습니다. [Quora challenge](https://www.kaggle.com/c/quora-insincere-questions-classification)는 quora의 질문을 sincere question과 insincere question으로 분류하는 과제입니다.

우선 필요한 패키지를 import 합니다.

~~~
import spacy
import pickle
import string

import numpy as np
import pandas as pd
from tqdm import tqdm

from spacy.lang.en.stop_words import STOP_WORDS
from spacy.lang.en import English
~~~

pandas를 이용해 train set과 test set을 불러옵니다.

~~~
train_df = pd.read_csv("./train.csv")
test_df = pd.read_csv("./test.csv")

print("Train datasets shape:", train_df.shape)
print("Test datasets shape:", test_df.shape)

>>> Train datasets shape: (1306122, 3)
>>> Test datasets shape: (56370, 2)
~~~

train set과 test set을 합치고 question과 target(0,1)을 각각 questions, target으로 저장합니다

~~~
# test_set 구분
test_df["target"] = 3

# concat으로 train set과 test set 합치기
df = pd.concat([train_df,test_df])

# question_text를 리스트로 저장
questions = list(df.loc[:,"question_text"])
target = list(df.loc[:,"target"])

# 행 번호 초기화
df = df.reset_index()

# index 열 삭제
del df["index"]

train_df["question_text"][0]

>>> 'How did Quebec nationalists see their province as a nation in the 1960s?'
~~~

이제 spacy parser를 불러와 문장(string)을 원하는 형태로 가공합니다. 저는 punctuation과 stopwords 모두 사용하고자 문장에서 제외하지 않았습니다.

~~~
# punctuation
punctuations = string.punctuation
# English stopwords
stopwords = list(STOP_WORDS)

# English parser를 호출
parser = English()

# tokenizer 정의, 필요에 따라 함수를 바꾸면 됩니다.
def spacy_tokenizer(sentence):
    mytokens = parser(sentence)
    mytokens = [ word.lemma_.lower().strip() if word.lemma_ != "-PRON-" else word.lower_ for word in mytokens ]
    mytokens = " ".join([i for i in mytokens])
    return mytokens
~~~

모든 문장을 tokenizer에 넣습니다.

~~~
tqdm.pandas()
questions = df["question_text"].progress_apply(spacy_tokenizer)
~~~

단어 사전을 만들어보겠습니다. key는 단어, value는 등장 횟수입니다.

~~~
# vocab이라는 이름의 딕셔너리를 만들고 각 단어의 개수 세기
# 단어가 없으면 새로 만듦
def build_vocab(sentences, verbose = True):
    vocab = {}
    for sentence in tqdm(sentences,disable = (not verbose)):
        for word in sentence:
            try:
                vocab[word] += 1
            except KeyError:
                vocab[word] = 1
    return vocab
~~~

띄어쓰기를 기준으로 단어를 구분해 vocab을 생성합니다. 위에서 각 토큰을 띄어쓰기 기준으로 .join했기 때문에 split()으로 단어를 구분합니다.

~~~
# 각 문장을 띄어쓰기 기준으로 잘라서 sentences에 넣기
sentences = questions.progress_apply(lambda x: x.split()).values

# vocab 생성
vocab = build_vocab(sentences)

print({k: vocab[k] for k in list(vocab)[:5]})

>>> {'how': 302704, 'do': 345489, 'quebec': 174, 'nationalist': 286, 'see': 15920}
~~~

모든 문장의 길이는 가장 긴 문장과 동일합니다. 빈 단어는 <PAD>로 채우고 처음 본 단어는 <UNK>로 채웁니다.
이를 위해 <PAD>와 <UNK>를 추가합니다.
~~~
word2ix = {"<PAD>":0, "<UNK>":1}

for i, v in enumerate(vocab):
    word2ix[v] = i+2

word2ix

>>> {'<PAD>': 0,
    '<UNK>': 1,
    'how': 2,
    'do': 3,
    'quebec': 4,
    'nationalist': 5,
    'see': 6,
    'their': 7,
    'province': 8,
    'a': 9,
    'nation': 10,
    ...}
~~~

이번에는 key를 index, value가 단어인 딕셔너리를 생성합니다.

~~~
ix2word = {word2ix[k]:k for k in word2ix.keys()}

ix2word

>>> {0: '<PAD>',
    1: '<UNK>',
    2: 'how',
    3: 'do',
    4: 'quebec',
    5: 'nationalist',
    6: 'see',
    7: 'their',
    8: 'province',
    9: 'a',
    10: 'nation',
    ...}
~~~

다음으로 question의 단어들을 index로 채운 list를 만들고 questions_ix에 저장합니다.

~~~
questions_ix = []

for question in questions:
    if len(questions) != 0:
        seq = []
        for word in question:
            seq.append(word2ix[word])
    else:
        seq = []
        seq.append(word2ix["<UNK>"])
    questions_ix.append(seq)

questions_ix[0:5]

>>> [[2, 3, 4, 5, 6, 7, 8, 9, 9, 10, 11, 12, 13, 14],
     [3, 15, 16, 9, 17, 18, 19, 2, 20, 15, 21, 22, 23, 17, 24, 25, 26, 14],
     [27, 28, 29, 30, 31, 14, 32, 29, 30, 33, 34, 14],
     [2, 3, 35, 36, 37, 38, 12, 39, 40, 14],
     [41, 42, 43, 44, 45, 46, 23, 9, 47, 48, 49, 50, 51, 12, 52, 14]]
~~~

모든 문장의 길이를 가장 긴 문장에 맞춰야 하므로 이를 계산합니다.

~~~
max_seq_length = max([len(seq) for seq in questions_ix])

print(max_seq_length)

>>> 159
~~~

pickle 확장자로 저장해 나중에 불러오기 편하게 만들어 줍니다.

~~~
save_data = {
    "questions": questions,
    "target": target,
    "questions_ix": questions_ix,
    "word2ix": word2ix,
    "ix2word": ix2word,
    "max_seq_length": max_seq_length
}

with open("quora_data.pickle", "wb") as f:
    pickle.dump(save_data, f)
~~~

pickle 데이터를 불러올 때는 pickle.load를 이용하고 딕셔너리 key를 이용해 불러옵니다.

~~~
with open("quora_data.pickle", "rb") as f:
    quora_data = pickle.load(f)

quora_data.keys()

>>> dict_keys(['questions', 'target', 'questions_ix', 'word2ix', 'ix2word', 'max_seq_length'])
~~~

~~~
questions = quora_data["questions"]
target = quora_data["target"]
questions_ix = quora_data["questions_ix"]
word2ix = quora_data["word2ix"]
ix2word = quora_data["ix2word"]
max_seq_length = quora_data["max_seq_length"]
~~~
