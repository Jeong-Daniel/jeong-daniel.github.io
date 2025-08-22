---
title: Pandas loc와 iloc차이
date:   2022-05-09 23:29:41 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

앞에서 loc와 iloc로 문제를 푼게 있었습니다. 이번에는 좀더 자세하게 알아보고자 합니다.

 

loc는 location의 약어로, 데이터 프레임의 행 또는 칼럼의 label이나 boolean array로 인덱싱하는 방법입니다. 즉, 사람이 읽을 수 있는 라벨 값으로 특정 값들을 골라오는 방법이라고 생각하면 됩니다. 

 

## df.loc[행 인덱스 값, 열 인덱스 값]

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210139695-92f0b477-d3f3-4416-a2d3-97bcab008329.png)

예를 들어서 우리는 index가 254고 LotArea의 값을 알고 싶습니다. 그러면 8400이겠지요

![loc](https://user-images.githubusercontent.com/85277660/210139700-23292a5b-d16a-4710-bcdb-fe14d7a0c159.png)

이렇게 우리가 읽을 수 있는 값으로 접근하게 됩니다.

그렇다면 하나의 값만 준다면 어떻게 될까요?

![trian loc](https://user-images.githubusercontent.com/85277660/210139710-d7eef248-5020-47a7-8694-4d6d319b2c33.png)

해당 행에 있는 값들을 모두 가져오게 됩니다.

![img1 slicing](https://user-images.githubusercontent.com/85277660/210139712-4a66aab7-cf47-4a49-a7a9-56e2b618da85.png)

참고로 이렇게 슬라이싱도 가능합니다.

여기서 loc는 불리언 값을 가지고 데이터를 가져올 수 있습니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210139719-16b1bb40-fb53-4654-94fb-20b395578989.png)

대충 LotFrontage값이 60이상인 것들의 불리언 값을 얻을 수 있습니다. 이를 그대로 적용하면 되는데요

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210139767-bd3eaf5c-eab0-4ccb-9850-ddbd8887ac34.png)


이렇게도 사용할 수 있습니다. 사실 대괄호안에 조건을 그대로 넣어도 무방합니다.

```python
X_train.loc[term_1|term_2]
X_train.loc[term_1&term_2]
```

그리고 이런 식으로 조건을 여러개 만들어서 | 은 or 조건을 만족하는 애들, &은 and 조건을 만족하는 것으로 더 걸어 줄 수 있습니다.

 

loc는 사람이 읽을 수 있다고 했습니다. 그러면 iloc는 컴퓨터가 읽기 편한 방식이라고 생각하시면 됩니다. 사용방법은 똑같으며 단지 문자대신 오르지 정수만 들어간다는 점 기억하시면 됩니다. 참고로 loc와는 달리 조건문을 동일하게 넣지 못합니다.

 

df.loc[행 인덱스 값, 열 인덱스 값]

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210139777-5a9d040c-9b13-40ef-ad05-53785761846e.png)

처음에 254인덱스와 LotArea라고 주었는데 간단하게 그 위치 값인 0과 4를 넣으면 똑같은 값을 얻을 수 있습니다. 슬라이싱 역시 전과 마찬가지로 사용하시면 됩니다.

![slicing result](https://user-images.githubusercontent.com/85277660/210139794-f106b595-2425-4573-8d86-06d636bb448f.png)

이렇게 슬라이싱을 이용하시면 3의 배수만 선택해볼 수 있습니다.