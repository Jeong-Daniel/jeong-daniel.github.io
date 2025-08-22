---
title: Pandas로 이상치 처리하기 quantile 사용
date:   2022-05-09 23:55:41 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

이번에는 quantile를 가지고 이상치를 처리해보도록 하겠습니다. 백분위수로 보통 IQR*1.5-Q1 ~ IQR*1.5+Q3을 기준으로 하고 이를 넘어가면 이상치로 판단합니다. 보다 큰 값 또는 작은 값들을 대체하거나 빼버리는 식입니다. 작업 1유형의 예시를 가지고 한번 해보겠습니다.

```python
import numpy as np
import pandas as pd
df1 = pd.read_csv('./mtcars.csv')
```

데이터를 불러오는 것을 시작을 합시다. 그중에서도 qsec열을 가지고 해보겠습니다.

```python
Q1_qsec = df1['qsec'].quantile(0.25)
Q3_qsec = df1['qsec'].quantile(0.75)
print(Q1_qsec, Q3_qsec)
```

![img1 quc](https://user-images.githubusercontent.com/85277660/210139966-71a7037d-12b2-4e6b-a230-1f0a515d98fe.png)

사용 방법은 간단합니다. 원하는 열에다가 quantile(백분위수)를 넣으면 알 수 있습니다. IQR의 경우 Q3-Q1이니 빼주면 되겠지요

```python
IQR = Q3_qsec-Q1_qsec
print(IQR)
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210139971-9be3aee8-8c5b-408e-81b3-b6a4393a0763.png)

이제 IQR도 알았으니, 조건식을 이용하면 이상치가 제거된 값을 구할 수 있습니다.

```python
data_IQR = df1[(df1['qsec']>Q1_qsec-IQR*1.5)&(df1['qsec']<Q3_qsec+IQR*1.5)]
data_IQR['qsec'].hist()
```

![img1 hist](https://user-images.githubusercontent.com/85277660/210140049-2d645268-2b3d-4b4f-b928-b093d2fbb609.png)

& and 문을 이용하여 하한값, 상한값을 걸어주면 우리가 원하는 값을 얻을 수 있습니다. 다만 주어진 데이터는 IQR기준으로는 이상치가 없기 때문에 처리를 하든 말든 결과는 비슷하지만 대충 이렇게 해본 다는 느낌으로 접근해보았습니다.

```python
data_IQR = df1[(df1['qsec']<Q1_qsec-IQR*1.5)|(df1['qsec']>Q3_qsec+IQR*1.5)]
data_IQR['qsec'].sum()
```

빅데이터분석기사 복원문제중에 조건에 맞는 이상값의 총 합 구하기가 있었는데 조건문에서 부호를 반대로 하고 &를 | or문으로 바꾼다음에 sum을 사용하면 간단하게 구할 수 있습니다. 여기서는 1개 밖에 안나오는 군요 저는 22.9 한개 나왔습니다.