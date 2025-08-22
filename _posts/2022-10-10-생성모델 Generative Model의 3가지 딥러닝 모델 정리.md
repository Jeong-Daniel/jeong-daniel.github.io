---
title: 	생성모델 Generative Model의 3가지 딥러닝 모델 정리
date:   2022-10-10 18:14:41 +0900
categories: [Computer_Vision, Study]
tags: [review, vision, ai, dl]
---

최근 컴퓨터비전/딥러닝 분야를 준비하면서 학문 그 자체에 대한 공부도 당연히 중요하지만 일단 용어정리부터 쭉 하자는 생각으로 다시 포스팅을 작성을 하였습니다. 이것만 쓰고 이력서 마저 쓰러 가야겠네요

시작에 앞서 생성모델에 대한 간단한 정의와 이것을 왜 사용하는지부터 천천히 봅시다. 무작정 배우는데 급급하다보면 핵심을 놓치게 되는데 그러면 나중에 한참 가서 그래서 내가 이거를 왜 하고 있는거지 라는 생각에 사로잡힐때가 많았거든요

[Background: What is a Generative Model?](https://developers.google.com/machine-learning/gan/generative)

일단 정의는 그렇다치고 인공지능  모델은 크게 두가지가 있는 것을 생각 해볼 수 있습니다.

문장 NLP와 관련된 기술이 있습니다.

첫번째 모델은 리뷰가 긍정인지 부정인지 판단할 수 있습니다. (판별모델)

두번째 모델은 상황을 설명을 해주면 기사를 써주는 모델이 있습니다. (생성모델)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211181150-63c62832-eebb-4cc5-bf4e-181833926df3.png)

출처 : [https://sites.google.com/site/machlearnwiki/bayesian-learning/saengseong-model](https://sites.google.com/site/machlearnwiki/bayesian-learning/saengseong-model)  


판별모델(Discriminative Model)이란 데이터 X가 주어졌을때 Label Y가 나타날 조건부확률 P(Y|X)를 직접적으로 반환하는 모델입니다.  

생성모델(Generative Model)이란 training data가 주어졌을 때 이 training data가 가지는 real 분포와 같은 분포에서 sampling된 값으로 new data를 생성하는 model을 말합니다.  

[https://thispersondoesnotexist.com](https://thispersondoesnotexist.com)

주소로 들어가면 GAN을 통해서 생성된 사람의 이미지를 랜덤하게 보여줍니다. 주소의 이름 역시 "이 사람은 존재하지 않는다" 입니다.

생성모델은 지도적 생성모델과 비지도적 생성모델로 나누어지고(이 부분은 넘어가겠습니다) 그리고 비지도적 생성모델에는 통계적인 방법과 딥러닝을 이용한 방법이 있는데 여기서 VAE, GAN, DDPM등이 등장하게 되고 이들을 간략하게 설명하고자 포스팅을 해봅니다.

### 1. AE, VAE(Variational Auto-Encoder) 변형된 오토인코더

VAE를 보기전에 먼저 오토인코더(Autoencoder)부터 살펴봅시다.

![encoder](https://user-images.githubusercontent.com/85277660/211181170-e331b98b-a0b6-4ee5-94ba-25d3b890d717.png)

구조 자체는 간단합니다. 입력을 출력으로 복사하는 신경망입니다. 하지만 입력과 출력의 결과가 완전히 똑같으면 안되기에 네트워크에 여러가지 방법으로 제약을 줘서 어려운 신경망으로 만들게 됩니다. 위 그림처럼 hidden layer의 뉴런 수를 input layer(입력층) 보다 작게해서 데이터를 압축(차원을 축소)하거나 입력 데이터에 노이즈(noise)를 추가한 후 원본 입력을 복원할 수 있도록 네트워크를 학습시키는 등 다양한 오토인코더가 있습니다.

이러한 제약들은 오토인코더가 단순히 입력을 바로 출력으로 복사하지 못하도록 방지하며, 데이터를 효율적으로 표현(representation)하는 방법을 학습하도록 제어합니다. 다른 딥러닝 모델의 경우 입력층과 출력층에 대한 이야기를 중심으로 하지만 오토인코더는 인코더와 디코더라는 말을 사용하는데 이 부분을 좀 더 봅시다.

인코더(encoder) : 인지 네트워크(recognition network)라고도 하며, 입력을 내부 표현으로 변환한다.

디코더(decoder) : 생성 네트워크(generative nework)라고도 하며, 내부 표현을 출력으로 변환한다.

입력 그 자체가 정답이 되고 Loss는 출력과 입력을 비교한 값으로 나타납니다.

이제 VAE를 봅시다. VAE는 [Auto-Encoding Variational Bayes](https://arxiv.org/pdf/1312.6114v10.pdf) 논문에서 제안한 오토인코더의 한 종류입니다. 그리고 오토인코더와는 목적이 다르기에 오토인코더를 다루지 않고 바로 VAE를 다루는 곳도 많습니다. 왜 다른지 봅시다.

논문 제목처럼 Bayes 베이지안에 대한 언급을 하는데 이는 확률적인 의미를 담고 있습니다.

> 베이지안 확률(Bayesian probability): 세상에 반복할 수 없는 혹은 알 수 없는 확률들, 즉 일어나지 않은 일에 대한 확률을 사건과 관련이 있는 여러 확률들을 이용해 우리가 알고싶은 사건을 추정하는 것이 베이지안 확률이다.

VAE는 확률적 오토인코더(probabilistic autoencoder) 즉, 학습이 끝난 후에도 출력이 부분적으로 우연에 의해 결정

VAE는 생성 오토인코더(generatie autoencoder)이며, 학습 데이터셋에서 샘플링된 것과 같은 새로운 샘플을 생성

오토인코더의 목적은 어떤 데이터를 잘 압축하는것, 어떤 데이터의 특징을 잘 뽑는 것, 어떤 데이터의 차원을 잘 줄이는 것 입니다. 반면 VAE의 목적은 Generative model으로 어떤 새로운 X를 만들어내는 것이다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211181199-a80f39c5-c33c-43c1-96dc-d8283b6fd7d2.png)

VAE의 인코더(encoder)는 평균 코딩 ​와 표준편차 코딩을 만든다. 실제 코딩은 평균이 ​이고 표준편차가 ​인 가우시안 분포(gaussian distribution)에서 랜덤하게 샘플링되며, 이렇게 샘플링된 코딩을 디코더(decoder)가 원본 입력으로 재구성하게 된다.

VAE는 마치 가우시안 분포에서 샘플링된 것처럼 보이는 코딩을 만드는 경향이 있는데, 학습하는 동안 손실함수가 코딩(coding)을 가우시안 샘플들의 집합처럼 보이는 형태를 가진 코딩 공간(coding space) 또는 잠재 변수 공간(latent space)로 이동시키기 때문이다. 이러한 이유로 VAE는 학습이 끝난 후에 새로운 샘플을 가우시안 분포로 부터 랜덤한 코딩을 샘플링해 디코딩해서 생성할 수 있습니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211181203-998a1cbe-c9fe-4014-9f86-826116e5097b.png)

위 그림도 결국 똑같은 의미 입니다.

어디까지나 해당 포스팅은 생성모델에 대한 찍먹 수준으로 볼 것이라 여기까지만 하고 넘어가겠습니다. 더 제세한 내용은 다른 게시글을 찾아보시길 바랍니다.

### 2. GAN(Generative Adversarial Network) 생산적 적대 신경망

맨처음 생성모델과 판별모델을 설명하기 위해서 가져온 그림 입니다. GAN 생산적 적대 신경망은 이 두 판별기라는 두 신경망을 경쟁적으로 학습시켜 성능을 개선시키는 신경망 모델 입니다. 판별기는 데이터셋의 원본데이터와 생성모델이 만들어내는 위조 데이터를 분석하는 역할을 합니다. 그러면 생성기는 판별기를 속이기 위해서 더 정교한 위조 데이터를 만들기 위해서 노력을 합니다.

![GAE daumcdn](https://user-images.githubusercontent.com/85277660/211181211-7336384a-9c6d-4f7b-82b8-49c5eeff3c5f.jpg)

보다 구체적으로, GAN은 아래와 같은 목적함수 V(D,G)를 이용하여 아래 수식처럼 minmax problem을 푸는 방식으로 학습하게 되며, 여기엔 GAN의 기본적인 학습내용이 모두 포함되어 있습니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211181222-98c50e7e-18b0-4890-9b2e-df8babe88fda.jpg)


처음의 생성기는 학습데이터를 전혀 보지못한 상태이기에 랜덤시드와 파라미터를 바탕으로 아무 출력을 만들어냅니다. 이때는 판별기 역시 위조 데이터를 제대로 구분하지 못합니다. 하지만 이진형태의 문제를 계속해서 풀어나가는 과정에서 점차 구분하는 능력을 갖추게되고 생성 모델은 판별기를 속였는가 속이지 못했는가를 학습하면서  간접적으로 정보를 획득하게 됩니다. 이렇게 두 모델이 경쟁적으로 학습을 하기에 생산적 적대 신경망이라고 합니다.

적대적생성망이 혁신적인 이유는 데이터 생성문제를 해결할 수 있을 것이라 보았습니다. 처음 다룬 오토인코더는 비지도학습을 수행한다는 점이 특별했지만 여전히 데이터를 필요했으며 자체적으로 데이터를 생성하는 것은 아닙니다. 물론 인코더가 생성하는 코드벡터를 난수 함수가 발생한 값으로 대체해서 디코더에 준다면 입력없는 데이터(Y)를 생성해볼 수 있지만 품질을 끌어올리기가 상당히 어렵다는 문제가 있습니다.

### 3. DDPM (Denoising Diffusion Probabilistic Models)

[https://paperswithcode.com/paper/diffusion-models-beat-gans-on-image-synthesis](https://paperswithcode.com/paper/diffusion-models-beat-gans-on-image-synthesis)

이 논문에서는 nonequilibrium thermodynamics(비평형을 이루는 열역학)로부터 고안된 잠재 변수 모델 중 하나인 diffusion probabilistic models를 제안한다. 이 모델은 high quality image synthesis를 수행 할 수 있다.

![models view](https://user-images.githubusercontent.com/85277660/211181240-cb2db2bb-8c4f-4a03-9033-32597402a973.png)

noising process의 역과정을 수학적으로 나타내서 역과정을 학습하는 방법이 DDPM입니다.

Diffusion process에서는 원래 이미지가 있을때 이것에 아주 조금(nearly infinitesimal) 노이즈화시키고, 이것을 다회 반복(논문 실험에서는 1000회)하여 원래 이미지와 거의 independent한 noise로 만듭니다. 신경망은 nearly infinitesimal한 diffusion process의 역과정인 reverse process의 coefficient를 학습하게 됩니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/211181244-40384668-7905-457d-91c4-2a7942d278dc.png)

간단히 결론부터 설명하면 DDPM은 주어진 이미지에 time에 따른 상수의 파라미터를 갖는 작은 가우시안 노이즈를 계속해서 더해나가는데, 최종적인 노이즈 덩어리는 원본과는 연관이 없는 수준까지 가게 됩니다. (노이즈는 normal distribution을 따름)

모델에다가 normal distribution을 따르는 랜덤한 노이즈를 주게 된다면 학습한 가중치를 따라서 원본(실제로 존재하지 않는)을 복원하게 됩니다. 노이즈를 통해서 원본을 복원했지만 실제 원본은 존재하지 않으니 이미지를 생성하것과 똑같은 효과를 얻게 됩니다.

![dalle](https://user-images.githubusercontent.com/85277660/211181247-21764128-93ca-4436-bb69-14a8804cde41.png)

최근 Novel AI, DALL·E 그림을 그려주는 AI가 화제가 되고 있는데 이런 방식을 이용해서 생성하는 것으로 알고 있습니다.

이미지 생성 모델은 인물이나 형태에 대해서 학습을 하고 키워드나 사진의 형태에 따른 가중치를 가지고 있습니다. 예를 들어서 위 그림처럼 아보카도 형상을 한 의자라고하면 아보카도와 의자에 대한 가중치를 가져와서 노이즈를 뿌리고 거기서 이미지를 복원(생성)해서 결과물을 보여주는 형식일 것입니다.

예전에 해당 글을 잠깐 보았을때는 그런가보다 하고 지나갔는데 다시금 정리하면서 보니 새롭네요 정말 공부할 것도 많습니다.

거의 찍먹수준으로 대충 이미지를 생성하는 3가지 모델, VAE, GAN, DDPM에 대해서 다루어 보았습니다. 더 자세한 수학적인 수식이나 이론은 논문이나 다른 게시글을 찾아보는 편이 좋을듯합니다.