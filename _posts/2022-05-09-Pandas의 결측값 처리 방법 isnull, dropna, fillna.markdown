---
title: Pandas의 결측값 처리 방법 isnull, dropna, fillna
date:   2022-05-10 16:29:41 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

데이터 분석을 할때 먼저 선행되어야 할 것으로 데이터 정제가 있고 대표적으로 결측값 처리에 대해서 다루게 됩니다. 이번 빅분기 실기를 준비를 하면서 Pandas를 통해서 결측값을 확인하고 이를 대체하거나 버리는 방법에 대해서 적어 보려고 합니다.

```python
import numpy as np
import pandas as pd

X_train = pd.read_csv("X_train.csv", encoding = 'euc-kr')
X_train.head()
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210139041-5d7587ef-1d1d-474b-8f5c-5f2764cafd15.png)

얼핏 보았을때 환불금액에 대해서 NaN이라고 값이 안들어가있습니다. 실제로 할때는 얼핏 보면 안되고 아래 명령어를 이용해서 확인을 해봅시다.

### 1. 결측값 확인하기 isnull() / isna()

![img1 example](https://user-images.githubusercontent.com/85277660/210139083-16421d7c-5649-4f45-ad60-4af8706cccd9.png)

간단하게 isnull이라는 명령어로 확인할 수 있지만 우리가 원하는 형태는 아닐 것입니다. 모든 값에 대해서 값이 있다면 False, 없다면 True로 반환을 해버립니다. isnull대신 isna역시 똑같이 작동합니다. 편하신대로 하나만 기억하시면 됩니다.

 

여기에다가 .sum()을 더붙여서 사용하면 간단하게 확인이 가능합니다.

![img1 train](https://user-images.githubusercontent.com/85277660/210139093-cac01723-f593-4ae6-9dcb-6f1bc5541ef0.png)

sum()의 경우 안에 숫자가 들어 있다면 모든 값을 더해서 반환을 하게 됩니다. 하지만 True/False가 있을경우 True는 1, False는 0으로 계산해서 다 더하게 됩니다. 즉 같이 isnull과 sum을 같이 사용하면 이렇게 값을 얻을 수 있습니다. 참고로 sum의 매개변수는 axis가 있으며 비어있다면 axis=0이 기본값으로 columns기준입니다.

 

여기서 더 궁금하신 분도 있을것입니다. 반대로 값이 없는 것 대신에 값이 있는 것을 알고 싶으시다면  notna를 사용하시면 됩니다.


![img1 id](https://user-images.githubusercontent.com/85277660/210139101-e465580e-ac04-4bd7-b4a4-98b81a05279f.png)


그러면 이렇게 반대로 값이 들어 있는 경우를 확인할 수 있습니다.

 

## 2. 결측값 버리기 dropna()
결측값을 다루는 방법중 해당 값을 버리는 것이 있습니다. 하지만 이는 좋지 못한 방법으로 값을 일괄적으로 버리게 되면 그만큼 학습하게되는 데이터가 줄어드니 모델을 제대로 학습할수 없을지도 모릅니다. 그렇기 때문에 결측값이 전체 데이터에서 무시할만큼 작다면 버려도 되겠지만 가급적이면 다른 값으로 대체해서 사용하는게 좋습니다.

![dropna](https://user-images.githubusercontent.com/85277660/210139156-bedbaec9-63a2-4d0a-bfe6-d62df0adf923.png)

dropna를 적용했을때 모습입니다. 3500개의 자료중 결측값이 하나라도 있는 index를 날려버리니 1205개로 줄어든 모습입니다. 다만 남은 데이터는 완전하다는 장점이 있습니다. 그리고 dropna역시 축을 설정할 수 있습니다. 기본값 0은 index를 처리하지만 1로 하게되면 열을 날려버리게 됩니다.

![dropna](https://user-images.githubusercontent.com/85277660/210139167-524eaafc-58f9-485c-acbe-56b1cf405556.png)

환불금액이 날라간것을 확인할 수 있습니다.

### 3. 결측값 대체하기 fillna()
이번에는 결측값을 대체하고자 합니다. 사용하는 방법은 간단합니다. 괄호안에 결측값대신 넣고자 하는 값을 집어 넣으면 됩니다.

![fillna](https://user-images.githubusercontent.com/85277660/210139175-9583dece-1a9f-4114-a39d-ee07e5512e0b.png)

fillna(0)을 입력해주니 NaN값들이 전부다 0으로 반환된 것을 확인 할 수 있습니다.

0대신에 평균값이나 다른 것도 넣을 수 있습니다.

![describe](https://user-images.githubusercontent.com/85277660/210139184-cbecf9aa-225d-4a92-8844-ad07e88c586a.png)

describe()를 사용하면 각 열들의 범위와 평균등을 알 수 있습니다. 여기서 평균값을 알아내서 넣는 방법도 있고

![fillna](https://user-images.githubusercontent.com/85277660/210139191-adf39135-e6d6-48dc-b24b-89d1def2025e.png)

아니면 이렇게 원하는 열에다가 mean()을 사용하시면 한줄로 처리되는 것도 확인 할 수 있습니다.

 

method{‘backfill’, ‘bfill’, ‘pad’, ‘ffill’, None}, default None


Method to use for filling holes in reindexed Series pad / ffill: propagate last valid observation forward to next valid backfill / bfill: use next valid observation to fill gap.

 

파라미터를 설펴보면 method라는 것이 있습니다. backfill, bfill의 경우 뒤에 있는 값을 채우겠다는 의미이며 pad, ffill의 경우 앞에 있는 데이터로 채우겠다는 의미 이며 None는 기본값으로 수행하지 않는 다는 뜻입니다.

 

쉽게 말해서 데이터의 순서가 1 - NaN - 2라고 했을때 backfill이라면 1 - 1 - 2가 되고 ffill을 사용하면 1- 2 - 2가 됩니다.제 생각에는 method는 아마 실기에 나오기 어렵지 않을까합니다. 그냥 이런게 있구나 하면 될거 같습니다.

 

다음번에는 이상치 처리에 대해서 해보겠습니다.