---
title: YOLO 학습을 위한 Darknet 설정하기(작성중)
date:   2022-06-21 10:48:30 +0900
categories: [Computer_Vision, Study]
tags: [opencv,vision]
---
*CUDA를 사용할 것이기 때문에 엔비디아 그래픽카드 권장

사용할 프로그램의 버전은
*버전마다 호환성이라던가 많이 다른데 그래도 자기 환경에 맞추어서 최신버전으로 진행하면 됩니다.
[MS 비쥬얼 스튜디오 2019](https://my.visualstudio.com/Downloads?q=visual%20studio%202019&wt.mc_id=o~msft~vscom~older-downloads)
*특별히 프로버전을 사용하는것이 아니라면 Community 2019 (version 16.11) 다운

[CUDA 11.2](https://developer.nvidia.com/cuda-11.2.0-download-archive)

[cuDNN 8.1.0](https://developer.nvidia.com/rdp/cudnn-archive)

[OpenCV 4.6.0](https://opencv.org/releases/)

CUDA와 비쥬얼 스튜디오가 설치되어 있더라도 모두 삭제하기를 권장합니다. 비쥬얼 스튜디오가 설치되어 있어야지만 설정이 되는 CUDA가 있는데 설치할때 순서가 바뀌면 설치가 안됩니다. 그리고 위에것 중에 최신버전이 있다한들 받지 마시고 버전을 꼭 맞춰서 진행하셔야 합니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210751666-d0f73ca3-3ee2-404d-be2c-6eb788f93645.png)

1. MS Visual Studio 2019를 설치를 해줍시다. 그리고 설치 옵션에서 C++를 이용한 데스크톱 개발을 선택

2.