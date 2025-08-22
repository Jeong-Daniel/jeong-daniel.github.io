---
title: 파이썬 패키지 관리 freeze requirements.txt에서 file @ 경로문제
date:   2022-09-21 11:50:46 +0900
categories: [Languages, Python]
tags: [python, module, library]
comment: true
---

파이썬에서 작업을 하다가 내가 사용하는 환경 그대로 백업을 하거나 다른 곳에서 받아볼때 환경 관리가 필요하다 라이브러리 버전도 동일할때 작동을 보장할 수 있는데 그래서 pip freeze를 이용해서 간단하게 할 수 있는데 여기서 문제가 발생합니다.

```
pip freeze > requirements.txt
```
pip freeze에서 패키지 목록을 볼 수 있고 이를 requirements.txt 파일로 export합니다. 당연히 파일 명은 마음대로 해도 됩니다.

```
pip install -r requirements.txt
```

여기서 -r은 파일을 읽겠다는 것이고 해당 리스트에 있는 파일을 읽어서 자동으로 설치를 해주는데

이렇게 끝나면 아름답지만 현실은 그렇지 않지요

![error massage](https://user-images.githubusercontent.com/85277660/211158476-261d46cd-1c1b-4ae3-bce5-adeba49dcbf5.png)

우리가 원하는 리스트는 "패키지명 == 패키지버전" 인데

왠 @ file:///C~ / file:///opt/conda로 하는 로컬 경로를 표시를 하게되고 이 파일을 가져다가 다른 곳에다가 설치를 하더라도 당연히 정상적으로 작동을 하지 않습니다. 제 로컬 경로를 띄운 것이기 때문이지요

저는 주로 아나콘다에다가 주피터노트북을 사용하는데 아마 여기 셋팅하는 과정에서 웹이 아니라 로컬에서 그대로 패키지를 받아서 오는 문제인거 같았습니다. 이럴때 pip freeze 대신 conda용도 있습니다.

 
### conda list 저장하기(export 하기)
```
conda list --export > packagelist.txt
```

![conda export](https://user-images.githubusercontent.com/85277660/211158538-fc9211fb-9806-48f6-804e-ee923f6c56ef.png)

그러면 pip freeze와는 사뭇 다르지만 패키지명과 버전을 확인할 수 있고 이를 사용하면 됩니다.

### conda list 불러오기(Import 하기, 새로운 가상환경에서 다시 install 하기)

```
conda install --file packagelist.txt
```

![conda error](https://user-images.githubusercontent.com/85277660/211158565-594e6987-34fc-47af-bde3-570eab0d996d.png)

그런데 문제는 이 방법은 설치가 보장 되지 않습니다. conda 패키지안에서 찾아서 설치를 하는 것은 문제가 없지만 제가 사용하는 opencv같은 경우에는 외부 pip에서 받아오게 되는데 이는 conda 패키지에서 인식을 못해서 설치 실패가 뜹니다.

 

 

그래서 저는 이렇게 했습니다 (노가다)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211158574-af796b46-01c2-44ea-9dc2-bde05c279ac9.png)

pip freeze 대신 pip list를 하면 이렇게 패키지와 버전이 뜹니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211158583-99a84de2-70eb-4a48-a1c3-c736a2fc3fe2.png)
![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211158586-da4cc3f5-4f4d-46e7-a18f-d93aee722ec8.png)

그러면 이것도 마찬가지로 > 를 입력해서 파일명을 지정하면 이렇게 txt 파일로 나오는데

 

이제 저거 다 뜯어 고치면 됩니다. (결국 이것도 텍스트 파일이니 파이썬으로 자동화툴을 만들어 볼 수도 있겠네요)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211158598-73869cef-93bb-4e8d-a45c-8f63c8095537.png)

노가다로 다 바꾸고

그대로 pip install -r 파일명을 입력하면

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211158605-34340a4d-a3f8-4b7c-bbc8-0d808015204b.png)

정상적으로 잘 설치 됩니다. 끗


혹시 버전에러가 있으면 해당 부분만 직접 손으로 수정해주면 됩니다. 저 같은 경우 이유는 모르겠는데 numpy 버전이 충돌 일어난다고해서 해당 부분 직접 수정했습니다. (이게 맞나...?)


예전에 쓴글인데 그냥 파이썬 코드 몇 줄만 쓰면 우아하게 바꿀 수 있을거 같은데 라는 생각이 드네요