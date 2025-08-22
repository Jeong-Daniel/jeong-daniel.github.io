---
title: 우분투 Vnc4server 패키지 설치 에러 해결방법 Package vnc4server is not available, but is referred to by another package
date:   2022-07-14 14:50:10 +0900
categories: [Tech, Ubuntu]
tags: [linux, server, cloud, remote]
---

AWS에 EC2 클라우드 컴퓨터를 띄우고 여기에다가 우분투를 가지고 장고를 셋팅하는 실습을 진행하고 있는데 GUI환경에서 원격 연결을 위해 Vnc4server 패키지를 설치 할려고하자 다음과 같은 에러가 나옵니다. 

![error massage](https://user-images.githubusercontent.com/85277660/210772075-4cdf51a7-f3b8-43f7-ba00-40678fc24fe0.png)

Package vnc4server is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or is only available from another source

[https://packages.ubuntu.com/](https://packages.ubuntu.com/)

우분투 패키지에서 vnc4server을 찾아봅시다.

![package vnc4server](https://user-images.githubusercontent.com/85277660/210772176-04edcfb7-d6a1-44a2-b8ce-b26ce1f2c17b.png)

bionic version and belongs to the universe repository.

해당 버전은 universe 저장소에 속해있는데 연결되지 않아서 발생한 문제로 보입니다. 해당 링크를 수동으로 설정합시다.

```bash
sudo vim /etc/apt/sources.list
```
vi든 nano든 텍스트 편집기로 sources.list를 수정합시다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210772447-5b06bd94-1625-4f37-9640-0d56e8db4ea0.png)

여기서 맨 아래에다가 

```
deb http://cn.archive.ubuntu.com/ubuntu/ bionic universe
```
해당 주소를 추가를 해줍시다.

 

그리고 저장을 하고 빠져 나와서 재부팅을 해줍시다.

 

*그런데 이렇게 텍스트 편집기 열어서 수정하는거 말고
```
sudo apt-add-repository 'deb http://cn.archive.ubuntu.com/ubuntu/ bionic universe'
```
이런식으로 입력이 가능한거 같더라

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210772535-7f63fda6-c5ed-440b-a68d-be8fdc24f2aa.png)

문제는 여기서 끝이 아니라는거...

```
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 3B4FE6ACC0B21F32
```

기본적인 레포지리가 아닌 다른 것을 추가할때 PUBKEY에러를 볼 수 있다. 

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210772615-56f9af6d-a22b-4581-9f0c-7d45cf9430c1.png)

대충 중간에 NO_PUBKEY ~~~~ 해서 뒤에 어떤 키가 필요한지 나오게 된다.

[http://pgp.mit.edu/](http://pgp.mit.edu/)

위 사이트에 들어가서 코드를 검색하는데 앞에 0x를 붙여야 한다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210772693-3a7a4cb0-54dd-4c0f-aa17-1a38d6413cb0.png)

그러면 이렇게 keyID값이 나오고 이 키값을 들고

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 키값
```

일일이 입력해줘서 키값을 넣으면 된다.

![final done](https://user-images.githubusercontent.com/85277660/210772814-87d1769f-1695-47a7-9d6c-a1070df154e9.png)

드디어 설치가 된다.

나는 그냥 장고를 AWS에 띄우고 싶었을 뿐인데..... 아주 예전에 리눅스 페도라가지고 공부한게 이렇게 도움이 될줄은 몰랐다.