---
title: Python itertools 내장 모듈 정리
date:   2022-08-15 14:49:30 +0900
categories: [Languages, Python]
tags: [python, module, library]
comment: true
---

[https://www.geeksforgeeks.org/python-itertools/](https://www.geeksforgeeks.org/python-itertools/)  

[https://docs.python.org/ko/3/library/itertools.html](https://docs.python.org/ko/3/library/itertools.html)  

Python에는 다양한 내장 함수를 지원을 합니다. 물론 이들은 코딩테스트에서도 대부분 문제없이 사용할 수 있기 때문에 기억을 하시면 굉장히 유용합니다. 특히 순열과 조합은 직접 만드는 것보다 내장함수를 사용하는게 훨씬 좋습니다.

하지만 이것 말고도 다양한 함수가 있는데 정리를 해보겠습니다.

```py
help(itertools)
```

시간이 나시면 help로 한번 읽어 보셔도 좋을 것입니다.

## 여러 이터레이터 연결하기
### chain
여러 이터레이터를 하나의 순차적인 이터레이터로 합치고 싶을 때 chain을 사용합니다. 

```py
it = itertools.chain([1,2,3],[4,5,6])
print(list(it))
```

![chain](https://user-images.githubusercontent.com/85277660/211013921-cd5134e5-72f7-4b17-ac79-825603046d7e.png)

코테같은 것 풀면은 두 리스트를 합치는 경우도 있는데 이를 이용하면 간단하게 만들 수 있으며 대게 속도도 더 빠릅니다.


### repeat
한 값을 계속 반복해 내놓고 싶을 때 repeat를 사용합니다. 이터레이터가 값을 내놓는 횟수를 제한하려면 repeat의 두 번째 인자로 최대 횟수를 지정하면 됩니다.

```py
it = itertools.repeat('Python', 3)
print(list(it))
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211014003-5f1443ec-47b0-48b4-85fa-13f1b2eb204b.png)

for문을 이용해서 append나 곱하기를 해서 넣는것보다 더 간결합니다.

### cycle

어떤 이터레이터가 내놓는 원소들을 계속 반복하고 싶을 때는 cycle을 사용한다.
```py
it = itertools.cycle([1,2])
result = [next(it) for _ in range(10)]
print(result)
```

![cycle](https://user-images.githubusercontent.com/85277660/211014049-0ccb1a97-bbf6-4491-8bbd-56b407bde2a5.png)

1과 2가 반복이 되는데 it는 이터레이터가 생성되기 때문에 next로 하나씩 생성이 됩니다.

### tee
한 이터레이터를 병렬적으로 두 번째 인자로 지정된 개수의 이터레이터로 만들고 싶을 때 tee를 사용한다. 이 함수로 만들어진 각 이터레이터를 소비하는 속도가 같지 않으면, 처리가 덜 된 이터레이털의 원소를 큐에 담아둬야 하므로 메모리 사용량이 늘어난다.

```py
it1, it2, it3 = itertools.tee(['하나','둘'],3)
print(list(it1))
print(list(it2))
print(list(it3))
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211014104-6a67f85e-f613-4fc6-9bfb-9814cb503b38.png)

### zip_longset

zip 내장함수의 변형으로 여러 이터레이터중 짧은 쪽 이터레이터의 원소를 다 사용한 경우 fillvalue로 지정한 값을 채워준다. 기본값은 None

```py
keys = ['하나','둘','셋']
values = [1,2]

normal = list(zip(keys, values))
print('zip:', normal)

it = itertools.zip_longest(keys, values, fillvalue='없음')
longest = list(it)
print('zip_longest', longest)
```

![zip_longset example](https://user-images.githubusercontent.com/85277660/211014144-72c23e19-fd76-4537-a94a-90b34c338b9c.png)

## 이터레이터에서 원소 거르기

itertools 내장 모듈에는 이터레이터에서 원소를 필터링할 때 쓸 수 있는 여러 함수가 있습니다.

### islice

이터레이터를 복사하지 않으면서 원소 인덱스를 이용해 슬라이싱하고 싶을 때 islice를 사용하면된다. 끝만 지정하거나, 시작과 끝을 지정하거나, 시작과 끝과 증가값을 지정할 수 있으며, islice의 동작은 시퀸스 슬라이싱이나 스트라이딩 기능과 비슷하다.

```py
values = [1,2,3,4,5,6,7,8,9,10]
first_five = itertools.islice(values,5)
print("앞에서 다슷 개:", list(first_five))

middle_odds = itertools.islice(values, 2,8,2)
print("중간의 홀수들: ",list(middle_odds))
```

![img1 odd](https://user-images.githubusercontent.com/85277660/211014233-d00519c2-9e58-4a04-8a70-477b9364db1a.png)

### takewhile

이터레이터에서 주어진 술어(predicate)가 False를 반환하는 첫 원소가 나타날때까지 원소를 돌려준다.(True를 반환하는 동안에는 원소를 반환함)

```py
values = [1,2,3,4,5,6,7,8,9,10]
less_than_seven = lambda x: x<7
it = itertools.takewhile(less_than_seven, values)
print(list(it))
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211014280-94c4adbe-ae84-456d-ba7f-9688e12e9a06.png)

### dropwhile

takewhile의 반대다. 이터레이터에서 중진 술어가 False를 반환하는 첫 원소를 찾을때까지 이터레이터의 원소를 건너뛴다.

```py
values = [1,2,3,4,5,6,7,8,9,10]
less_than_seven = lambda x: x<7
it = itertools.dropwhile(less_than_seven, values)
print(list(it))
```

![dropwhile](https://user-images.githubusercontent.com/85277660/211014309-236db58c-6617-488d-a032-2c6992032cfb.png)

### filterfalse

filter내장함수의 반대이다. 주어진 이터레이터에서 술어가 False를 반환하는 모든 원소를 돌려준다.

```py
values = [1,2,3,4,5,6,7,8,9,10]
evens = lambda x:x%2 == 0

filter_result = filter(evens, values)
print("Filter :",list(filter_result))

filter_false_result = itertools.filterfalse(evens, values)
print("Filter False:",list(filter_false_result))
```
![img filterfalse](https://user-images.githubusercontent.com/85277660/211014390-4d0ae128-4a31-42d0-815f-f03b44c5fc3b.png)


## 이터레이터에서 원소의 조합 만들어내기

itertool 내장 모듀렝는 이터레이터가 돌려주는 원소들의 조합을 만들어내는 몇가지 함수가 들어 있는데 combination, product, permutation은 생략 

### accumulate

파라미터를 두개 받는 함수(이항 함수)를 반복적용하면서 이터레이터 원소를 값 하나로 줄여준다.(fold 연산이라는 부류에 속함) 이 함수가 돌려주는 이터레이터는 원본 이터레이터의 각 원소에 대해 누적된 결과를 내놓는다.
```py
values = [1,2,3,4,5,6,7,8,9,10]
sum_reduce = itertools.accumulate(values)
print('합계', list(sum_reduce))

def sum_modulo_20(first, second):
    output = first + second
    return output % 20

modulo_reduce = itertools.accumulate(values, sum_modulo_20)
print('20으로 나눈 나머지의 합계:', list(modulo_reduce))
```

![accumulate](https://user-images.githubusercontent.com/85277660/211014479-1affa53f-ca07-4563-8678-9014cd354ad9.png)

합수를 적층적으로 더하는것을 볼 수 있고 또는 여기에 함수를 적용해서 다른 결과값을 얻을 수 있다.


코테를 많이 풀다보면 생각보다 이런 테크닉이 필요한데 이를 일일이 for문이나 조건을 거는 것보다 itertool을 적절하게 활용하면 쉽게 풀 수 있고 속도와 메모리 관리에서도 유용하다.

 


다시 정리하면 이터레이너나 제너레이터를 다루는 itertools 함수는 세가지 범주로 나눌 수 있다.

- 여러 이터레이터를 연결함

- 이터레이터의 원소를 걸러냄

- 원소의 조합을 만들어냄

 

파이썬 인터프리터에서 help(itertools)를 입력한후 표시되는 문서를 살펴보면 더 많은 고급함수와 추가 파라미터를 확인 할 수 있다.