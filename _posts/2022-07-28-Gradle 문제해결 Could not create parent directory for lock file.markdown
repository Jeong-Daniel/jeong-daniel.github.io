---
title: Gradle 문제해결 Could not create parent directory for lock file C \Program Files\Java\~
date:   2022-07-28 19:27:10 +0900
categories: [Tech, Spring]
tags: [java, gradle]
---

* 결말은 마지막에 있습니다. 바로 건너뛰고 그것만 봐도 됩니다.

![gradle fail](https://user-images.githubusercontent.com/85277660/210801064-f5d39ad8-1cee-4708-8b66-7e0cb3541416.png)


언제부터였나 설정을 잘못한건지 Gradle이 새로 빌드가 되지 않았다.

> Could not create parent directory for lock file C:\Program Files\Java\jdk-11.0.15.1\wrapper~

이제 공부하기 시작한 나로는 JDK문제인줄 알고 재설치도 해봤고 Gradle 폴더를 통째로 날리고 재설정도 하고 IntelliJ도 재설치 했지만 반응은 여전했다. 아마 지금 보고 있는 책이 한 3년쯤 된건다보니 코드가 대응하지 않아서인가 싶었지만 그러기에는 나는 그냥 새로 프로젝트를 만들었을 뿐인데 그것마저도 Gradle 빌드가 되지 않는거는 너무 심하지 않는가!


해서 어찌어찌 찾아보니 이렇게 설정을 하더라

![gradle setting](https://user-images.githubusercontent.com/85277660/210801144-0d338c53-7f3f-4fd3-b38f-fe21b0735582.png)


일단 Gradle 설정창은 다음과 같다.

설정 - Build, Execution, Deployment -> Build Tools -> Gradle

그리고 Gralde에서 Use Gradle from에서 어느 gradle를 사용할 것인지 세팅할 수 있고

기본값으로 된게 'gradle-wrapper.properties' file인데 이거는 

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210801229-0e8bd2fa-f17b-46dc-864d-e9acd5b8840f.png)

여기 프로젝트에 gradle - wrapper - gradle-wrapper.properties 파일안에 gradle에 대한 정보가 담겨 있다. 그리고 저렇게 설정된 경로는 

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210801265-447478a8-e382-4b35-80a5-9ac250379a49.png)

C드라이브 - 사용자 - 사용자 이름 - .gradle - wrapper - dists 폴더안에 저장이 된다.

이거를 지우고 새로 해봤는데!

지운거 까지는 좋았는데 이제는

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210801374-747515ea-74b0-49b5-bdc5-c21c1e3d7854.png)

생성도 안되고 안에 아무것도 남지 않았다.

일단은 다른 잘 작동하는 프로젝트에서 새로 빌드를 한 gradle를 집어 넣어보았지만 싱크가 되지 않는다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210801431-9968df7f-91c5-4618-be40-1442bbf822d1.png)

아니면 인터넷에서 받을 수 있는 Gradle 파일을 통째로 넣어보았지만 택도 없었다.

캐시 파일에 문제가 생긴건지 아에 포맷을 해야하는지 감이 안오는데... 이번 주말까지 좀 더 만져보고 안되면 포기해야 겠다.

라고 생각했는데

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210801462-036cc374-e9ad-400a-bd1d-2fdf6bdcdf4a.jpg)

혹시나 하는 마음으로 관리자 권한으로 실행하니

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210801480-4a474f71-53fc-4df2-a592-69ecca034e1e.png)

뭔가 바쁘게 움직이기 시작했고

![done](https://user-images.githubusercontent.com/85277660/210801520-72c2eba7-7d5b-40d3-817c-31d86c42f7aa.png)


작동을 한다......

그렇다!

Could not create parent directory for lock file

lock file의 상위 디렉터리를 만들 수 없습니다.

말그대로 일반 상태에서는 C드라이브에 쓰기 권한이 제한되어 있어서 gradle이 빌드하지 못했던것........

이걸로 몇시간을 날렸는지는 모르겠는데 그래도 해결 안된것보다야. ㅠ

![other error code](https://user-images.githubusercontent.com/85277660/210801614-ed7caa74-658c-4c85-8b53-8189e861236a.png)

> The specified Gradle distribution 'https://services.gradle.org/distributions/gradle-7.4.1-bin.zip' does not exist.

위와 같은 오류도 같은 방법으로 해결 가능합니다....

![permission](https://user-images.githubusercontent.com/85277660/210801690-881e2cb9-517d-404b-b013-e8c1484cc7b7.png)

인텔리는 번거롭게 켤때마다 관리자 권한으로 켜기보다 해당 파일을 찾아가서 호환성-관리자권한으로 이 프로그램 실행을 체크해서 자동으로 프로그램을 실행할때마다 관리자로 켜줍시다.