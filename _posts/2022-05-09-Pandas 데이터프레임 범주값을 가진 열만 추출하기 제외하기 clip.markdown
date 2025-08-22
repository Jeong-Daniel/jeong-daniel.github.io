---
title: Pandas 데이터프레임 범주값을 가진 열만 추출하기/제외하기
date:   2022-05-09 21:29:41 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

데이터를 전처리 할때 범주값과 이산값열을 분리를 하고 표준화를 수행하고 결측치를 처리했는데 좀 복잡하게 했습니다. 그런데 찾다보니 역시 필요한거는 이미 다 구현이 되어 있더라고요

```python
numeric_features = all_features.dtypes[all_features.dtypes != 'object'].index
```

저도 다른 사람이 한 것을 그대로 가져와서 사용하다보니.. dtypes를 두번 겹쳐서 object가 아닌 것에다가 index를 불러 왔는데 그렇게 복잡한것은 아니다보니 외우는데 문제가 없었는데 시간이좀 지나면 햇갈렸습니다.

 

그런데 찾다보니 Pandas에서 select_dtypes라고 이미 간단하게 함수가 만들어 져있습니다.
```python
numeric_feature = train.select_dtypes(exclude=["object"]).columns
```

exclude로 object를 제외하는 모습입니다. include도 마찬가지로 있으며 둘다 없이 대괄호만 사용하면 그 안에 해당하는 값만 가져오게되고 데이터프레임이 반환이 되는데 columns로 열의 이름을 바로 받아올 수 있습니다.

[https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.select_dtypes.html?highlight=select_dtypes](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.select_dtypes.html?highlight=select_dtypes)

마찬가지로 더욱 자세한 내용은 pandas를 참고해주시길 바랍니다.