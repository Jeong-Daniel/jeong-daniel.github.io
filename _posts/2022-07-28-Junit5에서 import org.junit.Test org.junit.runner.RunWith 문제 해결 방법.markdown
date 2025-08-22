---
title: Junit5에서 import org.junit.Test / org.junit.runner.RunWith 문제 해결 방법
date:   2022-07-28 20:37:10 +0900
categories: [Tech, Spring]
tags: [java, test, junit]
---

```java
import org.junit.Test;
import org.junit.runner.RunWith;
```

![package](https://user-images.githubusercontent.com/85277660/210802074-9c201ab4-bf85-47f0-a974-049824c6fbbc.png)

실습을 진행하다가 Test와 RunWith를 넣으면 위와 같이 빨간색으로 경고 표시가 나오고

![erore](https://user-images.githubusercontent.com/85277660/210802090-ca919e37-dabe-41a4-a7c0-8c09c5b389a3.png)

junit패키지에서는 해당 내역을 찾을 수 없다는 경고가 나온다.

junit4에서는 Test와 runner이 제공되었지만 지금 최신 버전인 5에서는 삭제되거나 변경되었기 때문이다.

그래서 지금은 이를 바꿔줘야 한다.

![dependencies](https://user-images.githubusercontent.com/85277660/210802164-8dcaf4d4-48f1-4de6-9388-29830c2def76.png)

처음에는 build.gradle에서 junit4버전으로 바꿔볼까도 생각했는데 intellij에서도 표시가 뜨지 않아서 다운된 버전을 사용하는대신 5를 그대로 써야겠더라

```java
//import org.junit.Test; junit4 구버전 대응
import org.junit.jupiter.api.Test;
//import org.junit.runner.RunWith; junit4 구버전 대응
import org.junit.jupiter.api.extension.ExtendWith;
```

Test는 중간에 jupiter.api.Test로
runner은 jupiter.api.extension.ExtendWith로 변경하면 된다. 

![test run](https://user-images.githubusercontent.com/85277660/210802244-d1a7e1d2-f630-4d66-b450-53b83ba89322.png)

이제 해당 내역에 대응해서 잘 작동한다!