---
title: utf-8 codec can't decode byte 0xa1 in position 18052 invalid start byte 해결 방법
date:   2022-06-24 13:51:00 +0900
categories: [Data_Sceince, Pandas]
tags: [pandas, data, python]
---

Pandas에서 csv파일이나 엑셀파일을 열때 'utf-8' codec can't decode byte 0xa1 in position 18052: invalid start byte 이런식으로 에러가 나올 수가 있습니다.

![utf-8 error code](https://user-images.githubusercontent.com/85277660/210752077-75ac3180-6493-4651-a395-51f07b3dd2bf.png)

참고로 여기서 0xa1이나 포지션 뒤의 20167 숫자는 컴퓨터나 파일마다 다를 수 있습니다.

큰 문제는 아니고 결국 코덱에서 인코드/디코드 문제이기 때문에 다른 인코딩을 사용하시면 됩니다.

utf-8 코덱으로는 디코더를 할수 없다는 에러이기 때문입니다.

![img1 cp949](https://user-images.githubusercontent.com/85277660/210752197-b3c63c29-d420-469f-a215-af31258aafa4.png)

이렇게 encoding 인자를 주어서 값을 cp949, euc-kr을 주면 한글 인코더로 열수 있습니다.

euc-kr과 cp949는 모두 한글 인코딩 방식으로 cp949가 euc-kr의 확장 버전입니다. 주로 cp949를 추천한다고 합니다.

이마저도 안된다고 하면 encoding='unicode_escape' 

유니코드 이스케프를 사용하면 정상적으로 파일이 열립니다.


참고글
[https://stackoverflow.com/questions/22216076/unicodedecodeerror-utf8-codec-cant-decode-byte-0xa5-in-position-0-invalid-s](https://stackoverflow.com/questions/22216076/unicodedecodeerror-utf8-codec-cant-decode-byte-0xa5-in-position-0-invalid-s)