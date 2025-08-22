---
title: SpringBoot 서버 포트 변경하는 방법
date:   2022-07-28 20:45:10 +0900
categories: [Tech, Spring]
tags: [java, srping, server, port]
---

스프링부트에서는 기본적으로 8080포트를 기본값으로 두지만 다른 자원이 이미 포트번호를 받아서 사용할 경우 아래처럼 WAS가 fail이 뜬다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210802448-578374d8-7c58-4778-b693-7cb48ab097f7.png)

이럴때 기본 포트 번호를 변경하면 된다.

![run code](https://user-images.githubusercontent.com/85277660/210802471-f98e855b-8208-4407-9fbb-000266b76da8.png)

스프링이니셜라이즈로 생성했을때 파일 디렉토리는 main-java-resources에 application.properties에 스프링부트 속성을 설정할 수 있는데 여기에서

server.port=포트번호

파일안에 아무내용이 없어도 상관없이 직접 지정하면 된다.

![spring run](https://user-images.githubusercontent.com/85277660/210802509-3d89bc7a-573f-4323-aba5-072f5a958375.png)

잘 작동하는 것을 확인!