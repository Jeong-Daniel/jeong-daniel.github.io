---
title: input 에러 ValueError not enough values to unpack (expected 2, got 0) 해결방법
date:   2022-05-13 12:20:46 +0900
categories: [Languages, Python]
tags: [python]
---

코딩테스트를 준비하면서 보는 교재에서 input은 시간이 느려서 시간초과가 나오니 파이썬 내장 모듈인 sys stdin을 사용하면 더 빠르게 input이 가능하다. 이것을 사용하라고 하는데 오히려 파일을 읽어나가거나, 자동으로 입력? 해주는것이 아닌 일반적인 노트북 환경에서는 에러를 일으켰다.

```py
n, m = map(int, input().split())
```

문제의 코드, 분명 오타는 없는데 여러번 반복해서 보아도 전혀 모르겠다.

에러코드와 input을 살펴보자

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210567287-958d2e1a-88c5-43ac-883f-57dbf2ae6017.png)

prompt에 argument가 존재하면, 개행 없이 표준 출력에 쓰여진다. input()은 입력으로부터 한 줄을 읽은 뒤, 그것을 (개행을 지우고) 문자열로 변환한 후 return 한다. EOF(End of file)을 읽으면 EOF에러를 일으킨다.

input 내장함수 > 개행문자를 제거 > 문자열로 변환 > 값 반환

sys.stdin file object 사용자의 입력을 받는 buffer를 만들어 그 buffer에서 읽어들이는 것

[https://docs.python.org/3/library/functions.html#input](https://docs.python.org/3/library/functions.html#input)

[https://docs.python.org/3.10/library/sys.html#sys.stdin](https://docs.python.org/3.10/library/sys.html#sys.stdin)


그러니까 input의 경우 아무내용이 없다면 EOF에러를 일으키지만 readline은 eof를 만나도 에러를 발생시키지 않고, 빈 문자열을 반환하게 된다.

대충 이렇게 공식 문서와 인터넷을 찾아보니 sys는 입력 버퍼를 만들어서 그 버퍼에서 읽어들인다는 것을 보고

아 이거 버퍼 클리어 문제구나 싶었다. C언어를 공부했다면 알겠지만 scanf 입력함수를 사용할때 자주 사용자를 괴롭히는 문제다. 그런데 이게 파이썬에서도 있는줄은 처음 알았다.


다시 설명하자면 버퍼가 이미 어떤 값(NULL을 포함한)이 점유하고 있으면 사용자에게 입력을 받기 전에 버퍼가지고 먼저 해결하려는 문제가 있다. ValueError: not enough values to unpack (expected 2, got 0) 문장 그대로 나는 받은 것이 없는데 너는 값 두개를 반환해달라는 것이다. 아니 너가 입력을 받아야지! 라고 해봐야 어쩌겠는가

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210567399-4d7553a3-e969-439e-91a0-e0786c8208cf.png)

아니나 다를까 input()을 입력하면 입력을 받지도 않았는데 빈값을 반환한다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210567439-0f2df172-9f18-471a-8aeb-5a1873e4d162.png)

del명령어로 input에 할당된 메모리를 해제하면 다시 정상적으로 입력이 된다.

![solution](https://user-images.githubusercontent.com/85277660/210567471-7c8b1a0a-e354-4cad-a2e5-b28e8d45a50e.png)

그리고 input = sys.stdin.readline 이거는 그냥 지워버리자