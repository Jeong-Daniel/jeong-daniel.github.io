---
title: 리눅스 failed to execute default web browser 문제 해결 방법
date:   2022-07-14 16:34:10 +0900
categories: [Tech, Ubuntu]
tags: [linux, web]
---

![fail error massage](https://user-images.githubusercontent.com/85277660/210773175-c9c2f736-8407-4558-9b44-38f59a378462.png)

장고 테스트 페이지를 띄울려고 원격 접속을 하는데 위 처럼 failed to execute default web browser 에러가 났다.

그냥 기본 값으로 설정된 웹브라우저가 없기 때문에 하나 설치를 해야하는거 같다. 분명 파이어폭스를 설치를 하기는 했는데 잘 모르겠다....

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo apt install ./google-chrome-stable_current_amd64.deb

google-chrome
```
위 명령어를 따로 입력하면서 리눅스용 크롬을 설치하자