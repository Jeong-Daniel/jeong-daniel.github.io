---
title: Pandas의 count()와 value_counts()의 차이
date:   2022-07-10 16:29:41 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

```python
df = pd.DataFrame(X_scaled_minmax)
df[df[0]>=0.5].count()
```

제가 무언가를 셀때 count()를 사용했었는데 판다스를 써본적이 있거나 다른 곳에서 value_counts()라는 것도 보신분이 있을 것입니다. 둘은 어떻게 다르고 어떻게 사용하는지 한번 보겠습니다.

 

일단 count() 부터 알아봅시다.

 

Parameters

axis : {0 or ‘index’, 1 or ‘columns’}, default 0

If 0 or ‘index’ counts are generated for each column. If 1 or ‘columns’ counts are generated for each row.

level : int or str, optional

If the axis is a MultiIndex (hierarchical), count along a particular level, collapsing into a DataFrame. A str specifies the level name.

numeric_only : bool, default False

Include only float, int or boolean data.

 

count의 매개변수는 총 3가지로 그중 axis만 알고 가셔도 됩니다. axis는 어떤 축을 기준으로 할 것인가 입니다. 0이라면 가로(row)를 기준으로 숫자를 세개되고 1이라면 세로(columns)가 기준이 됩니다. 직접 보는게 빠르겠네요


![pandas img](https://user-images.githubusercontent.com/85277660/210138875-4849f294-aeb9-425c-8cc9-0fb445df70a9.png)

count를 하니 시리즈 형태로 값을 출력합니다. 먼저 조건식으로 df를 특정 열을 뽑아오면서 columns의 이름이 0으로 재구성 되었습니다. 해당 조건을 만족하는 데이터가 0 컬럼에 9개 존재한다 입니다. 아무것도 쓰지 않을때는 기본값은 0입니다.

 

이제 축을 바꿔봅시다.

![pandas count](https://user-images.githubusercontent.com/85277660/210138903-8b579de9-4e7e-438c-8aae-06c50cbeba70.png)

index를 기준으로 계산을 하게 됩니다. 3번 index에 1개있고 5번 index에 1개 있고... 각 인덱스의 위치를 반환하게 됩니다.

![size](https://user-images.githubusercontent.com/85277660/210138907-728bd017-b79a-41af-b60d-5eaa75b5127a.png)

덤으로 size라는 매소드를 쓰면 간단하게 몇개 있는지 알려줍니다.

 

value_counts()를 살펴봅시다.

 

Parameters

subset : list-like, optional

Columns to use when counting unique combinations.

normalize : bool, default False

빈도와 비율중 선택을 합니다. 기본값은 빈도(False), True를 선택할 경우 비율을 반환합니다.

sort : bool, default True

정렬 기준에 빈도 여부를 물어봅니다. 기본값 False는 순서대로 호출하고 Ture는 빈도별로 정렬합니다.

ascending : bool, default False

정렬방식입니다. 기본값은 내림차순(False)이며 True로 할경우 오름차순입니다.

dropna : bool, default True

True면 NaN을 뭇하고 Falsue면 유일값에 NaN을 포함합니다.

 

매개변수는 5개로 1개의 옵션을 제외하면 4가지의 기본값이 있습니다. 


![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210138910-85f723f4-c842-4920-8545-f672258797e2.png)


사용해보시면 이렇게 해당 값들의 고유하개 몇개가 있는지 살펴볼 수 있습니다.

 

즉 다시 정리하자면 count는 특정 기준으로하는 크기를 알 수 있고 value_counts는 개별적인 값의 빈도를 알 수 있습니다.