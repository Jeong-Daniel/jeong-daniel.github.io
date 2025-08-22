---
title: Pandas 병합 merge 사용방법과 속성의 값 정리
date:   2022-07-10 16:29:41 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

파이썬 판다스 pandas.DataFrame.merge에 대해서 어떻게 사용하는지 살펴보고자 합니다.

```python
데이터프레임1.DataFrame.merge(데이터프레임2, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)
```

우리는 지금부터 데이터프레임1과 데이터프레임2를 한곳에다가 합치려고 합니다. 하지만 문제가 있습니다.

 

똑같은 인덱스와 속성값을 가지고 있다면 큰 문제가 없을 수도 있지만 언제나 같다는 보장은 되지 않습니다.

 

인덱스가 다를 수도 속성값이 다를수도 심지어 인덱스는 같은데 속성값이 중복될 수도 있습니다. 그럴 경우에 우리는 merge에 있는 여러가지 기능을 사용하면서 조절 할 수 있습니다. 기본적으로 위에 적어둔 값은 생략하면 기본값으로 반환하게 됩니다.

how = {‘left’, ‘right’, ‘outer’, ‘inner’, ‘cross’}, 기본값 ‘inner’

어떤 형식으로 병합할 것인가?

left(right): 왼쪽(오른쪽) 프레임의 키값만 가져옵니다. 키의 순서를 유지합니다. SQL left outer join 유사합니다.  

outer: 두 프레임의 키 합집합을 사용합니다. 키를 사전순으로 정렬합니다. SQL full outer join 유사합니다.  

inner: 두 프레임의 키 교차를 사용합니다. 왼쪽 키의 순서를 유지합니다. SQL inner join 유사합니다.  

cross: 두 프레임에서 데카르트 곱을 만들고 왼쪽 키의 순서를 유지합니다.  

on = label or list 기본값 None

조인할 열 또는 인덱스를 설정합니다. on이 기본값이 None이고 인덱스를 병합하지 않는 경우 기본적으로 두 DataFrame의 열 교차로 설정됩니다.

 


left_on(right_on) = label or list, or array-like 기본값 None

오른쪽(왼쪽) DataFrame에서 조인할 열 또는 인덱스입니다. 또는 리스트거나 왼쪽(오른쪽) DataFrame 길이가 됩니다. 이렇게 처리된 배열은 열처럼 다루게 됩니다.

 

left_index(right_index) = bool, 기본값 False

왼쪽(오른쪽) DataFrame의 인덱스를 조인 키로 사용하고자 합니다. 만약 여러 인덱스를 가진 MultiIndex인 경우 합치고자 하는 DataFrame의 키 수(인덱스 수)와 일치해야 합니다.

 

sort =  bool, 기본값 False

결과 DataFrame에서 조인 키를 사전순으로 정렬합니다. False인 경우 조인 키의 순서는 조인 유형에 따라 다릅니다.

 

suffixes = list-like, 기본값 (“_x”, “_y”)

합칠때 열의 접미사를 설정하게 됩니다. 기본값으로 설정해둘 경우 속성값에다가 _x, _y가 붙어서 나오게 됩니다. 

 

copy, indicator, validate

이거는 생략해도 될거 같아서 넘어가겠습니다. 궁금하신 분들은 마지막에 첨부해둔 주소에 나와 있습니다.


```python
df1 = pd.DataFrame({'lkey': ['foo', 'bar', 'baz', 'foo'], 'value': [1, 2, 3, 5]})
df2 = pd.DataFrame({'rkey': ['foo', 'bar', 'baz', 'foo'], 'value': [5, 6, 7, 8]})
```

실습을 해보겠습니다. 지금부터 위의 두 df1과 df2를 합쳐보고자 합니다.

![example_1](https://user-images.githubusercontent.com/85277660/210137826-d6af2e68-3dce-47eb-b9c8-30a1c540c282.png)

```python
df1.merge(df2, left_on='lkey', right_on='rkey')
```

우리는 df1과 df2를 합칠 것인데 왼쪽에다가는 1key를 오른쪽에다가는 rkey를 인덱스로 잡겠다는 의미입니다.

![example_2](https://user-images.githubusercontent.com/85277660/210137833-5d5cb13f-56e6-4849-986c-7b53af722c03.png)

```python
df1.merge(df2, left_on='lkey', right_on='rkey', suffixes=('_left', '_right'))
```

![example_3](https://user-images.githubusercontent.com/85277660/210137842-e03d928f-ff77-4e83-b9cb-8ae79768bad5.png)

위에다가 접미사를 붙여주니 _left와 _right가 붙은 것을 확인 할 수 있습니다.

```python
df1.merge(df2, left_on='lkey', right_on='rkey', suffixes=(False,False))
```

똑같이 겹치(여기서는 value)는 열이 있으면 예외가 발생합니다.

데이터를 새로 만들어 봅시다.

```python
df1 = pd.DataFrame({'a': ['foo', 'bar'], 'b': [1, 2]})
df2 = pd.DataFrame({'a': ['foo', 'baz'], 'c': [3, 4]})
```

![example_4](https://user-images.githubusercontent.com/85277660/210137857-ab35c31c-c226-4097-95c5-67e639a32e3c.png)


```python
df1.merge(df2, how='inner', on='a')
```

inner은 교집합 인덱스를 기준으로 만들게 되고 on은 특정 열을 기준으로 병합한다는 뜻입니다.

여기서 공통으로 들어있는 것은 a이기 때문에 다른 것을 넣으면 오류가 납니다.

![img1 merge](https://user-images.githubusercontent.com/85277660/210137874-6b43fca0-bc6e-4cb8-b4ad-7f73b6100d08.png)

```python
df1.merge(df2, how='left', on='a')
```

![merge result](https://user-images.githubusercontent.com/85277660/210137881-3df3fabf-c552-47cb-b18d-d59e16939e87.png)

왼쪽 df1의 프레임을 기준으로 가져오게 됩니다. 그렇기 때문에 baz가 아니라 bar이 나오고 c열의 두번째 컬럼은 없기 때문에 NaN을 반환합니다.

```python
df1 = pd.DataFrame({'left': ['foo', 'bar']})
df2 = pd.DataFrame({'right': [7, 8]})
```

![img1 merge](https://user-images.githubusercontent.com/85277660/210137895-895f2923-b8b6-4885-a0de-15467f99fdfb.png)

이 상황에서는 같은 데이터가 없지만 병합을 할 수 있습니다.

```python
df1.merge(df2, how='cross')
```

![result imge](https://user-images.githubusercontent.com/85277660/210137911-3df8e1a3-a9c0-4c7b-87c7-92068b64113f.png)

요렇게 크로스를 넣으면 foo-7, foo-8, bar-7, bar8 두개 두개 였으니 데카르트 곱을 하면 값이 4개가 튀어나옵니다.

 

참고 자료 : [판다스 공식 문서](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html)