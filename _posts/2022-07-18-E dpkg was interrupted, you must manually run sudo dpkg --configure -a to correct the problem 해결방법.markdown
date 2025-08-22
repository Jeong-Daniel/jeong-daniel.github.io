---
title: E dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem 해결방법
date:   2022-07-18 16:22:10 +0900
categories: [Tech, Ubuntu]
tags: [linux, server]
---

![error massage](https://user-images.githubusercontent.com/85277660/210788673-5f78eb93-4544-4e39-a8f5-76a14800434c.png)

장고와 MySQL을 연결하기 위해서 패키지가 하나 필요해서 설치를 할려고 하는데 위와 같은 에러가 나왔다.

```
E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.
```

일단 그대로 매뉴얼대로 \<sudo dpkg --configure -a\>를 입력하고 다시 설치 명령어를 쳤다.

![img1 dpkg](https://user-images.githubusercontent.com/85277660/210789976-4eb549a4-cf25-40b3-881f-5ee67b93aa58.png)

지난번에 구글 크롬 깔다가 뻑나서 강제종료 했는데 이 부분이 문제가 생긴거 같다.

![unpack](https://user-images.githubusercontent.com/85277660/210789998-00b97ac8-84c5-4ab6-8485-0a9d0f86e637.png)

그럼 그렇지... 재설치해도 문제는 똑같다. 20%에서 멈추고 진행이 되지 않는다.

결국 문제는 구글 크롬설치?로 가는거 같다.

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo apt install ./google-chrome-stable_current_amd64.deb
```

우분투에서 크롬을 설치하는 방법은 위 명령어를 차례로 입력하면 되지만

그래도 여전히 설치는 실패했고 다른 것은 설치할 수 없었다. 그러면 지워보자

```
sudo dpkg --remove --force-remove-reinstreq google-chrome-stable
```

이거는 잘 작동된다!

sudo apt-get update로 다시 최신화를 하고

![ubunt update](https://user-images.githubusercontent.com/85277660/210790151-60da5161-f542-4614-b215-4d11a769c9a5.png)

```
sudo apt-get -y install libmysqlclient-dev
```

다시 libmysqlclient를 설치해보자

![result](https://user-images.githubusercontent.com/85277660/210790208-bdcdbf7e-690b-427b-977a-b5887c91ab53.png)

설치 끝!

결론 : 패키지 꼬임