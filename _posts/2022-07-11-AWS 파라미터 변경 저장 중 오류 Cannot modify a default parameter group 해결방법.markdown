---
title: AWS 파라미터 변경 저장 중 오류 Cannot modify a default parameter group 해결방법
date:   2022-07-11 14:56:00 +0900
categories: [Tech, AWS]
tags: [sql, cloud, server]
---

이번에 새로 AWS 학습을 시작하면서 기본 설정부터 막혔다. AWS RDS의 DB 파라미터를 수정해야하는데

![error masseage](https://user-images.githubusercontent.com/85277660/210769928-b780a210-7530-4932-aef1-a752b20b4082.png)

저장 중 오류: Cannot modify a default parameter group. (Service: AmazonRDS; Status Code: 400; Error Code: InvalidParameterValue; 쭉 나왔다.

당연히 위 사진에서 답이 나와있다. 파라미터 그룹의 default 기본값은 수정 할 수 없는데 내가 여기로 들어가서 설정하려고 한것이었다....

![parmeter group](https://user-images.githubusercontent.com/85277660/210769989-26e1216b-c4d3-4a54-8e53-6cd66f1c2c93.png)

파라미터 그룹으로 들어가서 새로 그룹을 생성하고

![parameter group create](https://user-images.githubusercontent.com/85277660/210770103-f3c67e77-cdac-488a-8c69-9ece9b469c44.png)

아무 내용이나 적으면 된다.

![done img](https://user-images.githubusercontent.com/85277660/210770175-0e4cd6a5-46b0-471d-ae01-28945b6fda1e.png)

이제 적용이 된 것을 확인 할 수 있다.

사소하더라도 쭉 기록하면서 공부해야지