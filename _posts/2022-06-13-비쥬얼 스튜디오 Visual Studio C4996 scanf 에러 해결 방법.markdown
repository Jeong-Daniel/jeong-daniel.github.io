---
title: 비쥬얼 스튜디오 Visual Studio C4996 scanf 에러 해결 방법
date:   2022-06-13 11:34:46 +0900
categories: [Languages, C]
tags: [c,coding]
---

마이크로소프트 비쥬얼 스튜디오 13버전 이상에서 scanf를 그냥 사용하시면 다음과 같은 에러와 함께 컴파일러 실패를 합니다.

```
error C4996: 'scanf': This function or variable may be unsafe. Consider using scanf_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.
오류 C4996: 'scanf': 이 함수 또는 변수는 안전하지 않을 수 있습니다. 대신 scanf_s를 사용하는 것이 좋습니다. 지원 중단을 비활성화하려면 _CRT_SECURE_NO_WARNINGS를 사용하세요.
```

scanf는 보안상의 문제 때문에 scanf_s 사용하는 것을 권장하는데요 하지만 교재나 공부자료가 오래된 것이 많아서 지금 당장은 scanf를 사용하는 편이 편할 수 있습니다. 해결하는 방법은 에러를 죽이고 그대로 실행하면 됩니다.


에러 해결 방법 정리

### 1. #define _CRT_SECURE_NO_WARNINGS 입력
```c
#include <stdio.h>
#include <stdlib.h>
#define _CRT_SECURE_NO_WARNINGS
```
코드 헤더부분에다가 stdio.h같은 라이브러리를 불러올때 define으로 _CRT_SECURE_NO_WARNINGS를 지정을 해줍시다.


### 2. SDL검사를 안 한다 설정
![setting](https://user-images.githubusercontent.com/85277660/210571761-79517806-1c3b-4373-94b9-f51e9078e5b1.png)

프로젝트의 속성을 들어가신 다음

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210571821-eda8b876-fd25-4a05-a868-207ad9961835.png)

C/C++ > 일반 > SDL 검사에서 아니오(/sdl-)을 체크를 해줍시다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210571843-baea5a5d-f9f6-4e0f-9859-919db319daf2.png)

그리고 C/C++ > 전처리기에 들어가셔서

전처리기 정의에다가
```c
<_CRT_SECURE_NO_WARNINGS>
```
를 뒤에다가 추가해주시면 됩니다. 다만 앞뒤에다가 세미콜론 ; 을 붙이셔야 합니다. ;_CRT_SECURE_NO_WARNINGS; 가 되겠네요

여기까지 하시면 보통 정상적으로 scanf를 사용하고도 컴파일러가 잘 실행이 될것입니다.

아래는 그래도 작동이 안된다면 한번 해보시면 됩니다.


### 3. 솔루션 프로젝트를 만들 때 SDL검사 체크 해제하기
![img1 SDL check](https://user-images.githubusercontent.com/85277660/210571973-68dd04a7-eeb1-4841-aa83-d15584a6df4f.jpg)

### 4. #pragma warning(disable : 4996) 추가
```c
#pragma warning(disable : 4996) 
```
![pragma warning](https://uer-images.githubusercontent.com/85277660/210572028-644b5a7f-5cce-4c09-b06b-d0da7b2375b3.jpg)