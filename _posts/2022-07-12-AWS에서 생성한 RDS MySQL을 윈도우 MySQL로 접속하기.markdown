---
title: AWS에서 생성한 RDS MySQL을 윈도우 MySQL로 접속하기
date:   2022-07-12 16:04:10 +0900
categories: [Tech, AWS]
tags: [sql, cloud, server]
---

AWS에서 제공하는 서비스중 데이터베이스가 있습니다. 물론 여기서 데이터베이스를 생성하면 원격으로 접속이 가능한데 이를 MySQL DB를 생성한다음 윈도우 MySQL WorkBranch를 이용해서 접속해보겠습니다.

![Connections](https://user-images.githubusercontent.com/85277660/210770761-e8ad582e-f099-44ca-99d0-a83eae9cdd6d.png)

MySQL Connections에 추가 버튼을 눌러주고

![Connections Name](https://user-images.githubusercontent.com/85277660/210770785-d43c87aa-8cc0-48ee-8f8c-2e714ef19b97.png)

Connection Name는 아무거나 넣으시면 됩니다.

![connect and security](https://user-images.githubusercontent.com/85277660/210770828-9c7a3694-6067-4963-b7b7-d857584580bf.png)

Hostname은 AWS에서 DB 대시보드에 들어가서 연결 & 보안의 앤드포인트 주소를 집어 넣으면 됩니다. 포트 번호도 마찬가지로 밑에 나와 있습니다.

유저이름은 RDS에서 DB를 생성할때 지정한 마스터 이름을 사용하시면 됩니다.

![connetions setting](https://user-images.githubusercontent.com/85277660/210770967-87d07c94-dfc1-401b-8aca-21e69ce2b8a6.png)

그리고 Password에 Store in Valut에 들어가서

![setup paswored](https://user-images.githubusercontent.com/85277660/210771046-b436490d-95c5-4025-b9ec-3210e46ff37d.png)

비밀번호를 입력합시다.

![fail workbench](https://user-images.githubusercontent.com/85277660/210771114-09498cf8-5029-4c11-bff8-82501a62d36d.png)

이때 연결에 실패를 한다면 퍼블릭 엑세스 가능을 확인합시다. 내부가 아닌 외부 접속 여부를 확인해야 합니다.

![acces setting](https://user-images.githubusercontent.com/85277660/210771176-3bd581fe-5b82-4a0a-b6cb-33df02265d93.png)

이때 퍼블릭 액세스를 아니요 체크해다면 수정을 합시다.

![database](https://user-images.githubusercontent.com/85277660/210771228-c6227ba2-03b5-431f-a305-70b98d41ab49.png)

데이터베이스 대시보드에서 수정을 클릭하고

![public access true](https://user-images.githubusercontent.com/85277660/210771278-8df3ee60-e37a-4ef1-9b56-900d0c362710.png)

퍼블릭 액세스 가능으로 체크를 합시다. 물론 보안에는 취약해지는 것이 맞기에 다른 방법을 사용 할 수 있습니다.

![master paswwer setting](https://user-images.githubusercontent.com/85277660/210771320-4c830a5d-6958-4c92-a098-e3d461f14930.png)

마찬가지로 마스터 암호도 수정 할 수 있습니다.

![setting db instance](https://user-images.githubusercontent.com/85277660/210771422-c832cc38-b43f-4bd2-b784-af01484828b1.png)

그리고 즉시 DB인스턴스를 수정해줍시다.

여기서 끝이 아니라 보안 규칙을 하나 수정을 해줍시다.

![db dashboard](https://user-images.githubusercontent.com/85277660/210771458-14a909e7-b79e-4202-bb27-425451f9b302.png)

DB대시보드에 보안 VPC보안 그룹을 선택해서

![inbound rule](https://user-images.githubusercontent.com/85277660/210771482-af3b686f-8400-4cd7-9299-e1578eb22fdb.png)

아래 인바운드 규칙에 수정을 누르고

![add rule](https://user-images.githubusercontent.com/85277660/210771506-87a8caaf-8dd3-4a39-a653-25d0460c9631.png)

규칙을 추가한다음 유형에 MYSQL/Aurora 소스는 0.0.0.0/0으로 지정해줍시다. 지역에 무관하게 접속이 가능해집니다.

![mysql connetion alter](https://user-images.githubusercontent.com/85277660/210771527-4f60bcda-9064-4778-91bf-0d5b00a43178.png)

다시 돌아와서 접속을 하면 위와 같이 Successfully made the MySQL connection이 완성됩니다.

![done reuslt](https://user-images.githubusercontent.com/85277660/210771556-a07d3d64-0db4-4714-a913-601ab8c739f1.png)

여기서 자유롭게 쿼리문을 넣고 작업하시면 됩니다.