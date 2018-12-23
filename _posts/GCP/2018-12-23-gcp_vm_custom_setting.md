---
layout: post
title:  "Google Cloud Platform(GCP)에서 아나콘다, 주피터 노트북, 가상환경 설치 및 설정"
date:   2018-12-04 21:00:00
author: entiff
categories: GCP
---

## 들어가기 전

GCP에서는 이미 tensorflow와 pytorch 최신버전 그리고 이에 맞는 CUDA, cuDNN가 설치된 이미지를 배포하고 있습니다. Boot disk 설정에서 OS images를 보면 현재(18.12.23) pytorch 1.0.0/cuda 10.0, tensorflow 1.12.0/cuda10.0 이미지 등이 있습니다. 그리고 instance를 켜놓는 동안 계속 과금이 됩니다. 따라서, 로컬에서 테스트를 미리 해보고 GCP에서 jupyter notebook을 사용하는 것이 좋습니다. 그리고 **작업이 끝난 뒤에는 반드시 instance를 꺼야 합니다.** 사용하지 않는 동안 instance를 계속 켜두면 요금 폭탄을 맞을 수 있습니다.

이번 글은 GCP에서 본인이 원하는 OS image가 없는 분, jupyter notebook을 사용하고자 하는 분들을 위해 작성합니다.
GCP가 한글로 나와서 글의 내용을 따라가기 어려우신 분들은 `User preferences - Language & region`에서 Language를 영어로 바꾸면 됩니다.

그럼 시작하겠습니다.

## VM(Virutal Machine) 생성하기

[GCP](https://cloud.google.com/)에 접속해 `go to console`을 클릭합니다.


![GCP1](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp1.PNG?raw=true)


프로젝트를 원하는 이름으로 설정하고 `NEW PROJECT`를 누릅니다.


![GCP2](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp2.PNG?raw=true)


이제 `Compute Engine - VM instances`를 눌러 VM 관리창으로 들어갑니다.


![GCP3](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp3.PNG?raw=true)


`Create an instance`를 누르면 다음과 같은 창이 뜹니다. 밑줄 친 부분을 원하는 설정으로 변경합니다.


![GCP4](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp4.PNG?raw=true)


`Customize`를 누르면 다음과 같은 창 뜨고 표시된 부분을 원하는 설정으로 변경합니다.


![GCP5](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp5.PNG?raw=true)


밑으로 내려가서 Firewall에 Allow HTTP traffic, Allow HTTPS traffic에 체크합니다. Disks 옵션에서 Delete boot disk when instance is deleted를 해제해 instance를 지우더라도 disk를 지우지 않도록 설정합니다.


![GCP6](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp6.PNG?raw=true)


예시 VM 설정은 다음과 같습니다.

- **Name: instance-101**  
- **Region: us-west1**  
- **Zone: us-west1-b**  
- **Machine type: 8 vCPU, 30GB memory, 1 GPU(Tesla K80)**  
- **Boot disk: Ubuntu 16.04**

이제 IP와 방화벽 설정을 해야 합니다.

우선 IP 설정을 먼저 합니다. `VPC network - External IP addresses`로 들어갑니다.


![GCP7](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp7.PNG?raw=true)


Ephemeral(임시)를 static(고정)으로 변경합니다.


![GCP8](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp8.PNG?raw=true)


이제 방화벽 설정을 하러 `VPC network - Firewall rules`로 들어갑니다.


![GCP9](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp9.PNG?raw=true)


`Create Firewall Rule`을 누르면 다음과 같은 창이 뜹니다. 여기서 Source IP ranges에 `0.0.0.0/0`, Protocols and ports에는 Specified protocols and ports, tcp를 선택하고 원하는 1~4 자릿수의 port number를 설정합니다. 이미 있는 ports는 피해서 설정합니다.


![GCP11](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp11.PNG?raw=true)


설정이 모두 끝났습니다. instance에 접속합니다. `Compute Engine - VM instances`로 들어가 instance를 켭니다.


![GCP10](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp10.PNG?raw=true)


[Cloud Shell](https://cloud.google.com/shell/docs/) 또는 Google Cloud Sdk로 instance에 접속할 수 있습니다. Google Cloud SDK로 접속하겠습니다.

## Google Cloud SDK 설치

window 환경에서 Google Cloud SDK 설치는 [이곳](https://cloud.google.com/sdk/docs/quickstart-windows)을 참고했습니다. installer를 다운받아 설치하고 gcloud init를 입력합니다. 나머지 설정은 문서에 따라 차례차례 입력합니다.

## Google Cloud SDK로 instance에 접속

Google Cloud SDK를 실행하고 `gcloud compute ssh [instance name]`로 instance에 접속합니다. 저는 instance-101이라는 이름으로 만들었기 때문에 `gcloud compute ssh instance-101`로 접속합니다.

처음 instance에 접속할 때 registry key를 등록하라는 문구가 뜹니다. 가볍게 '예'를 누릅니다.
성공적으로 접속하면 다음과 같이 뜹니다.


![GCP12](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp12.PNG?raw=true)


## Anaconda 설치

이제 아나콘다를 깔아봅시다.
`https://repo.continuum.io/archive/` 여기서 본인의 instance os에 맞는 최신 버전 아나콘다 링크 주소를 복사합니다.

`wget [링크 주소]`로 파일을 가져옵니다.

```
wget https://repo.continuum.io/archive/Anaconda3-5.3.1-Linux-x86_64.sh
```

`bash [파일]`를 실행하고 설치 순서에 따라 진행하면 됩니다.

```
bash Anaconda3-5.3.1-Linux-x86 64.sh
```

archive 뒤가 파일명입니다.
여기까지 하면 다음과 같은 문구가 뜹니다.

```
In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
''>>>''
```

겁먹지 말고 enter를 계속 누릅니다.
계속 누르다 보면 다음과 같은 문구가 뜹니다.

```
cryptography
    A Python library which exposes cryptographic recipes and primitives.

Do you accept the license terms? [yes|no]
[no] >>>
```

yes라고 입력하고 enter를 누릅시다.
이제 다음과 같은 문구가 나옵니다.

```
Anaconda3 will now be installed into this location:
/home/test/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/test/anaconda3] >>>
```

enter를 누릅니다.

/home/test/anaconda3 경로에 아나콘다가 설치됩니다.
설치가 모두 완료될 때까지 기다립니다.

```
Do you wish the installer to initialize Anaconda3
in your /home/test/.bashrc ? [yes|no]
[no] >>>
```

yes를 입력합니다.

```
Do you wish to proceed with the installation of Microsoft VSCode? [yes|no]
'''>>>'''
```

VSCode를 설치할지 묻고 있습니다. no를 입력합니다.

`source ~/.bashrc` 라고 입력하면 드디어 gcp instance에 아나콘다 설치가 끝납니다.

## Jupyter Notebook 설치

이제 instance에서 주피터 노트북을 실행할 수 있도록 간단한 설정을 해보겠습니다.

우선 `conda install notebook`으로 주피터 노트북을 설치합니다.

그리고 jupyter notebook config 파일이 있는지 확인합니다.

```
ls ~/.jupyter/jupyter_notebook_config.py
```

없으면 `jupyter notebook --generate-config` 를 입력해 파일을 생성합니다.

이제 이 config 파일을 수정하겠습니다. `vi` 명령어로 편집합니다.

```
vi ~/.jupyter/jupyter_notebook_config.py
```

그러면 다음과 같은 화면이 뜹니다.


![GCP13](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp13.PNG?raw=true)


i를 누르면 화면 아래에 --INSERT--라고 뜨며 편집모드로 들어갑니다.
이제 다음과 같이 입력합니다.

```
c = get_config()
c.NotebookApp.ip = '[external IP address]'
c.NotebookApp.open_browser = False
c.NotebookApp.port = [Port Number]
```

external IP address는 VPC network - External IP addresses에서 본인 instance의 external address를 입력하면 됩니다.

```
예시
c = get_config()
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 101
```

esc를 누르고 :wq를 입력한 뒤 enter를 누르면 파일 편집 모드에서 나옵니다.

이제 떨리는 마음으로 주피터 노트북을 실행해봅시다.

다음과 같이 입력합니다.

```
jupyter-notebook --no-browser --port=[PORT-NUMBER]

예시
jupyter-notebook --no-browser --port=101
```

URL을 복사하고 port number의 콜론(:)앞에 external ip address를 입력하고 접속합니다.

다음과 같이 jupyter가 실행되면 성공한 것입니다.


![GCP14](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp14.PNG?raw=true)


이제 vm instance에서 원하는 패키지를 설치하고 사용하시면 됩니다.

## jupyter notebook kernel 추가

프로젝트마다 vm을 생성해 위와 같은 과정을 반복하면 시간이 매우 오래걸립니다.

아나콘다를 이용해 가상환경을 만들고 주피터 노트북에서는 kernel을 추가해 사용하면 하나의 vm에서 여러개의 프로젝트를 진행할 수 있습니다.

우선 vm 명령창에서 가상환경을 생성합니다.

```
conda create --name [가상환경 이름] [설치할 패키지]

예시
conda create --name entiff python=3.6
```

생성한 가상환경 리스트를 `conda info --envs`를 통해 확인합니다.

`conda activate [가상환경 이름]`으로 가상환경을 활성화합니다.

```
conda activate [가상환경 이름]

예시
conda activate entiff`
```

비활성화는 `conda deactivate`입니다.

다음 그림과 같이 본인이 생성한 가상환경 이름이 인스턴스 옆에 보이면 성공적으로 가상환경이 활성화된 것 입니다.


![GCP15](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp15.PNG?raw=true)


이제 가상환경에 jupyter notebook을 설치합니다.

```
conda install ipykernel
```

그리고 jupyter notebook에 가상환경 kernel을 추가합니다.

```
python -m ipykernel install --user --name [VirtualEnv] --display-name "[KenrelName]"

예시
python -m ipykernel install --user --name entiff --display-name "pytorch"
```

이제 다시 jupyter notebook으로 접속해서 다음과 같이 추가된 커널을 확인할 수 있습니다.


![GCP16](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gcp16.PNG?raw=true)


이번 글에서는 GCP VM에 아나콘다, 주피터 노트북 설치 그리고 가상환경 커널 추가에 대해서 알아보았습니다. 이후에는 본인이 원하는 버전의 라이브러리를 설치해 사용하면 됩니다.


### 참고 자료

- [Running Jupyter Notebook on Google Cloud Platform in 15 min](https://towardsdatascience.com/running-jupyter-notebook-in-google-cloud-platform-in-15-min-61e16da34d52)
- [jupyter notebook에 가상환경 kernel 추가하기](https://medium.com/@5eo1ab/jupyter-notebook%EC%97%90-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-kernel-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0-ed5261a7e0e6)
