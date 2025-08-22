---
title: Missing optional dependency 'xlrd' 문제해결 방법
date:   2022-05-08 09:15:46 +0900
categories: [Languages, Python]
tags: [python]
comment: true
---
엑셀 파일을 집어 넣으면

## (1) 셀 선택 모드 (Command Mode)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210078491-937b9af9-a367-4777-aa95-457d6bedb5cb.png)

ImportError: Missing optional dependency 'xlrd'. Install xlrd >= 1.0.0 for Excel support Use pip or conda to install xlrd.

이런식으로 에러 메세지가 출력 되는데 말그대로 xlrd에 종속되어 있는 문제이기 때문에 pip나 conda를 이용해서 설치해달라는 겁니다.

 
 ![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210078709-340453ce-e8cf-4a28-b5a0-3f839cd13aac.png)

아나콘다 환경들어가서 설치하시거나

 
conda install xlrd 또는

주피터노트북에다가 바로 !pip install xlrd 라고 치셔도 됩니다.

주피터노트북에서 어떤 커맨드를 입력할때 종종 앞에다가 ! 느낌표를 붙이는데 이는 콘솔창(터미널)일때와 똑같은 방식으로 사용하라는 의미입니다. 아니라면 cmd prompt를 실행시켜서 느낌표만빼고 설치해도 됩니다.