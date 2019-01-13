---
layout: post
title:  "Google Cloud Platform(GCP)에서 딥러닝 환경(아나콘다, 주피터 노트북, 딥러닝 라이브러리 설치) 구축하기"
date:   2018-12-04 21:00:00
author: entiff
categories: GCP
---


## 우분투 CUDA 9.2, cuDNN 7.1 설치

이제 vm에 cuda 9.2와 cuDNN 7.1을 설치해보겠습니다.

CUDA 9.2는 NVIDA driver version 396이 필요합니다.
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-396 nvidia-modprobe
```

위 코드를 한줄 씩 실행합니다. 시간이 꽤 걸립니다.

nvidia-smi를 입력해 설치가 제대로 됐는지 확인합니다. 다음과 같은 창이 뜨면 성공적으로 설치된 것입니다.

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 396.54                 Driver Version: 396.54                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla K80           Off  | 00000000:00:04.0 Off |                    0 |
| N/A   28C    P0    55W / 149W |      0MiB / 11441MiB |    100%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+

```

nvidia driver를 설치했으니 이제 cuda 9.2를 설치해봅시다.

[이곳](https://developer.nvidia.com/cuda-92-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal)에서 base installer를 다운 받습니다.

sudo sh cuda_9.2.148_396.37_linux.run

https://medium.com/@zhanwenchen/install-cuda-9-2-and-cudnn-7-1-for-tensorflow-pytorch-gpu-on-ubuntu-16-04-1822ab4b2421

gcloud compute scp [D://Quora/train.csv/train.csv] shwksl101@entiff:
