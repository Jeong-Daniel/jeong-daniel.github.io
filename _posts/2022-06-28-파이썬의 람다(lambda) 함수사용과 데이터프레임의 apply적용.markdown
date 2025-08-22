---
title: 파이썬의 람다(lambda) 함수사용과 데이터프레임의 apply적용
date:   2022-06-28 11:28:00 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

[표현식 - python 3.10.5 공식 문서](https://docs.python.org/ko/3/reference/expressions.html)

파이썬은 쉬운 언어라고 하지만 사실 그렇게 쉬운 언어는 아닙니다. 어떤면에 있어서는 C언어의 포인터보다 까다롭기도 하고 어떤 언어 하나를 완전히 잘쓰고 마스터 한다는 거는 굉장히 어려운 일이네요

저는 람다를 그렇게 좋아하지는 않습니다. 다른 구분 없이 한줄에 쭉 쓰다보니 나중에 코드를 보면 가독성이 떨어져서 다시 꼼꼼하게 봐야 했는데 데이터를 다루면서 apply와 함께 쓰면 편리한 것을 보고 다시 정리하러 왔습니다.

일단 파이썬 공식 문서에서는 표현식 부분에 정리되어 있습니다. 람다 함수 또는 람다 표현식(형식)이라고 하네요

 
> 람다는 이름 없는 함수를 만드는 데 사용됩니다. 표현식 lambda parameters: expression 는 함수 객체를 줍니다. 이 이름 없는 객체는 이렇게 정의된 함수 객체처럼 동작합니다
 
다시 표현하면 람다는 일회용 함수

![lambda func](https://user-images.githubusercontent.com/85277660/210752842-48db3d4b-fd2b-4181-a6b4-7552f1da3576.png)

```py
def is_even(x):
    return x % 2 == 0

is_even = lambda x : x % 2 == 0
```

위 두함수는 동일하게 동작을 합니다. 다만 람다의 경우 일회용이니 따로 객체를 만들지 않으면 사라집니다.

파이썬의 리스트에 맵함수를 적용하는 것처럼 판다스의 데이터프레임에서는 apply 함수를 적용할 수 있습니다.

apply함수는 DataFrame 칼럼에 복잡한 연산을 간단하게 만들어 줍니다. 특히 apply() 함수는 간단한 경우 lambda() 함수를 적용할 수 있으며, 복잡한 경우 사용자 정의 함수를 적용할 수도 있습니다.

함수를 만들어서 apply에 적용해도 되고 아니면 apply에 lambda로 일회용 함수를 사용해도 된다는 뜻

![img1 view](https://user-images.githubusercontent.com/85277660/210753015-a8325eed-ded7-4f14-899c-8416eb3caa0a.png)

예제로 볼 코드는 HS품목 코드입니다. 6자리가 기준인데(정확히 한국식 HS는 10자리) 살펴보면 0번부터 시작을 하는데 엑셀 파일 등에서는 숫자로 인식하는 바람에 한자리가 없습니다. 이때 맨 앞에다가 0을 붙여 주어야 한다고 합니다.

```py
def five_string(x):
    a = len(x)
    if a==5:
        x = '0'+str(x)
    return x
```
이렇게 숫자가 5자리일 경우에 0을 앞에 붙여주는 함수를 작성을 하고

```py
new_df['Code'] = new_df['Code'].apply(five_string)
```

apply로 적용을 합시다. 이때 한번 뷰를 보여주고 사라지기 때문에 적용을 하면

![result view](https://user-images.githubusercontent.com/85277660/210753159-d61ba3a5-75cd-4e25-9a14-20b40c103872.png)

101번을 보면 앞에 0이 붙은 것을 확인 할 수 있네요 이번에는 람다 함수를 사용해봅시다.

```py
new_df['Code'] = new_df['Code'].apply(lambda x: str(x) if len(x)==6 else '0'+str(x))
```
사실 이래서 제가 람다함수를 잘 사용 안했습니다.

```py
apply(lambda 매개변수: 첫번째 반환값 if 조건문 else 조건을 만족 하지 않을때 반환값)
```
if 앞에다가 if를 만족할때 반환값을 else뒤에다가 조건을 만족하지 않을때 반환값을 적어 놓으시면 됩니다. 결과는 동일합니다.