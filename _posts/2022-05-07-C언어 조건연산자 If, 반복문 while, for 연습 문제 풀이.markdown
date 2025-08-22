---
title: C언어 조건연산자 If, 반복문 while, for 연습 문제 풀이
date:   2022-05-07 09:50:46 +0900
categories: [Languages, C]
tags: [c,coding]
---
### 1에서부터 표준 입력으로 받은 양의 정수까지의 합을 출력하는 프로그램을 작성하시오.

![ 1에서부터 표준 입력으로 받은 양의 정수까지의 합을 출력하는 프로그램을 작성하시오.](https://user-images.githubusercontent.com/85277660/209969076-bb812204-30d5-4eaa-a905-3723cc866363.jpg)

```c
#include  <stdio.h>
int main() 
{ 
 	int num,i,sum; 
 	printf("양의 정수를 입력하세요 : "); 
 	scanf("%d",&num);

	for(i=1,sum=0;i<=num;i++) 
 	{ 
 		sum=sum+i; 
 		printf("%d + ",i); 
 	} 
 	printf("\b\b= %d",sum); 
}
```

변수는 3개를 선언하고, 입력받을 수 num, 전체 합을 계산할 sum, 수를 카운팅 할 i 3개를 선언한다.

for문을 이용해서 i를 1부터 sum까지 반복 수행하게 한다. +기호의 경우 if문 등을 사용해서 +와 =등으로 나눌 수 있지만

귀찮아서 백스페이스로 처리했다.


### 1에서 100까지의 정수 중에서 2,3,5,7의 배수를 제외한 수를 한 행에 10개씩 출력하는 프로그램을 작성하시오

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969177-8b6a316d-0fb9-4caa-867c-027e7286e076.jpg)

```c
#include  <stdio.h>
int main() 
{ 
	int i,j; 
	for(i=1,j=0;i<=100;i++) 
	{ 
		if(i%2!=0&&i%3!=0&&i%5!=0&&i%7!=0) 
	{ 
	printf("%2d ",i); 
	j++; 
	if((j%10)==0) 
		printf("\n"); 
	} 
	} 
}
```

사실 여기서는 여러 가지 방법을 사용할 수 있는데, 2,3,5,7의 배수라면 나누었을 때 0이 될 것인데 그거를 부정하면 수를 고를 수 있다.

if(i%2!=0&&i%3!=0&&i%5!=0&&i%7!=0) 사실 나는 아무 생각 없이 각각 부정했는데


if(!(i%2==0&&i%3==0&&i%5==0&&i%7==0)) 이렇게 2,3,5,7의 배수이다를 부정해주어도 되고


또 아니라면 컴퓨터는 참과 거짓을 나타낼 때 거짓(0)과 참(1)의 값으로 환원하는 것에 착안해여서

if(i%2&&i%3&&i%5&&i%7) 이라고 해주어도 된다.

어차피 2,3,5,7의 배수가 아니라면 나머지가 1 이상일 거니 조건 안에는 1 이상의 수가 들어갈 것이고, 이는 참이나 다름없다.

 

거기에 j라는 변수를 하나 더 선언해서 해당 수행을 할 때마다 1씩 카운팅 해서 10으로 나누었을 때 0이면 줄 내림을 시행한다고 명시해준다. 정확히는  j는 0으로 선언하고 9가 되면 10번째가 되니 줄내림을 하게 만들었다.


### 표준 입력으로 입력한 정수에서 각각의 자리에 해당하는 수를 반대로 출력하는 프로그램을 do while문을 이용하여 작성하시오.

![표준 입력으로 입력한 정수에서 각각의 자리에 해당하는 수를 반대로 출력하는 프로그램을 do while문을 이용하여 작성하시오.](https://user-images.githubusercontent.com/85277660/209969235-ede88016-92db-4194-b596-ea99a275c62d.jpg)

참고로 do while문은

 

do{

반복할 내용

}while(반복하는조건)

이다.

 ```c
 #include <stdio.h>
int main() 
{ 
int i,j; 
printf("정수 입력 : "); 
scanf("%d",&i); 
j=10; 
do 
    { 
     j=i%10; 
     printf("%d",j); 
     i=i/10; 
    }while(i!=0); 
printf("\n"); 
}
```
답지 안 보고 풀려고 했는데 도저히 모르겠던 게 어떻게 하면 자릿수를 사전에 알지 못하는데 수행하는가 였는데, 답지 보고 바로 이해...

변수는 두 개면 충분하다. 입력받을 수와 출력할 수

i에다가 숫자를 입력을 해두고, 이를 10의 나머지를 j에다가 넣어 준다.

그러면 j는 1의 자리 숫자가 들어가게 된다. 그리고 바로 출력

그런 다음 i를 10으로 나누는데 정수니까 소수점 이하를 버리게 되고 10의 자리가 1의 자리로 이동하게 된다. 다시 j에다가 넣고 출력하는 거를 반복해서 i가 0이 되면 끝!..... 답지 보고 조금 허탈했던 문제

/는 몫을 %는 나머지를


혹시 여기에다가 무한루프를 추가하고 특정 수를 넣었을 때 빠져나오게 하고 싶다면

while(1) or for(;;) 을 통해서 무한 루프로 만들어 주고

if(i==특정값) 
break;

을 이용해면 구현이 된다.

### 다음 수식과 내용을 참고로 해당하는 x와 y값을 출력하는 프로그램을 작성하시오.y = 3*3 + 2*2 + x + 5, x는 5에서 10사이 0.5씩 증가하도록

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969322-dd8bae0a-104f-43db-ad1a-34f4f739ca1b.jpg)

```c
#include  <stdio.h>
int main() 
{ 
double x,y; 
for(x=5.0,y=0;x<=10;x=x+0.5) 
  { 
  y=(3*3+2*2+x+5); 
  printf("%.1f=3*3+2*2+%.1f.2+5\n",y,x); 
  } 
}
```

쉽다고 만만하게 봤다가 이상하게 헤맨 문제, 값이 정상적으로 출력되지 않았는데 가만 보니 계속해서 정수일 때(int, %d)만 연습하다 보니 실수일 때를 잊어 먹었다. 0.5씩 증가하니 정수가 아니라 실수로 계산을 해줘야 한다.

 
### 다음 조건을 만족하는 프로그램을 작성하시오원금이 1,000,000인 경우, 예치 기간을 1년에서 10년까지 매년 말에 받을 총금액을 출력연단위 단리 이자는 4.5%만기 시 총수령액은(단리) = 원금(1+4.5%*예치기간))

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969381-79dd392e-2c48-45eb-a09a-de49a0b46e24.jpg)

```c
#include  <stdio.h>
int main() 
{ 
double money=1000000,earn; 
int year; 
earn=money*0.045; 
for(year=1,money;year<=10;year++) 
  { 
  money=money+earn; 
  printf("%02d년 말에 받을 총금액 : %.0f \n",year,money); 
  } 
}
```

이거는 문제가 좀 잘못된게 없지 않아 있다. '만기 시 총수령액은(단리) = 원금(1+4.5%*예치기간))' 이 부분 말이 이상하게 돼있는 거 같은데, 저거보고 짜다가 헤맸다. 그냥 이자는 단리 이므로 10년간 변하지 않는다. 맨 최초에 계산을 해주고 for 같은 반복문을 이용해서 매년마다 이자를 더해주고 연도도 같이 10년까지 더해주면 된다.


### 다음 조건을 만족하는 프로그램을 작성하시오 원금이 1,000,000인 경우, 예치 기간을 1년에서 10년까지 매년 말에 받을 총금액을 출력년단위 복리 이자는 4.5% 만기 시 총수령액은(복리) = 원금(1+4.5%)*예치기간) 함수 pow(a,b) 이용 #include 

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969423-4aab12ea-7a40-4e68-9e97-5ed25216f54f.jpg)

pow(a,b) 함수는 a^b이다.

```c
#include  <stdio.h>
#include  <math.h>
int main() 
{ 
double money; 
int year; 
for(year=1;year<=10;year++) 
  { 
  money=1000000; 
  money=money*pow(1.045,year); 
  printf("%02d년 말에 받을 금액%.0f\n",year,money); 
  } 
} 
```

사실 코드가 좀 이상하게 짜여지기는 했는데 for문을 이용해서 금액*(1+4.5%) 매번(매해)마다 곱해주면 된다.

다시봐도 코드가 좀 이상하기는 한데.... 변수를 하나 더 쓸걸 그랬나?

### 표준 입력으로 받은 9 이하의 정수로 구구단을 출력하는 프로그램을 작성하시오

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969487-a46131be-acdf-46f1-a6b6-442b12d3206b.jpg)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969521-23b4b8e2-787b-4017-aa8d-065d733668bd.jpg)

```c
#include  <stdio.h>
int main() 
{ 
int j,i; 
printf("9 이하의 정수를 입력하세요: "); 
scanf("%d",&j); 
if(j>9||j<1) 
	printf("숫자를 다시 입력해주세요"); 
else
{
  for(i=1;i<=j;i++) 
    { 
    if(i%3!=0) 
    printf("%d ",i); 
    } 
} 
}
```
별거 아닌데 좀 길다. 딱히 조건에는 주지 않았지만 1~9를 벗어나는 수를 입력하면 다시 입력하게 하고 싶어서

if(j>9||j<1) (9초과, 1미만의 경우)

printf("숫자를 다시 입력해주세요"); 를 넣었다.


구구단이니 3을 제외하고 출력하면 될것이다. 이를 if(i%3!=0) 라고 했는데 if(i%3)라고해도 무방하다.

이유는 위에 적어둔 것과 동일

### 다음 식을 참고로 썹씨 온도(C)를 화씨온도(F)로 변환하는 프로그램을 다음과 같은 출력이 되도록 작성하시오1. F=(9.0/5.0)*C + 322. 섭씨온도가 -60부터 140까지 20씩 증가, 이때의 화씨온도를 구하여 출력하는데 온도는 정수 형태로 출력

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969565-efe1f268-f990-4932-93d2-1bad5fe501b2.jpg)

```c
#include  <stdio.h>
int main() 
{ 
double c=(-60),f; 
printf("온도 출력 프로그램 \n"); 
for(;c<=140;c=c+20) 
  { 
  f=(9.0/5.0)*c+32; 
  printf("섭씨온도 : %3.0f, 화씨온도 : %3.0f\n",c,f); 
  } 
}
```

뭐 간단하다. 수식넣고 20씩 더하면서 루프 돌리면 되는 거라. 다만 정수 형태로 출력하라고 했는데 9.0/5.0이 있어서 실수로 입력받고 소수점을 다 지워버렸다. 

### 1부터 n까지의 합 중에서 10000을 넘지 않는 가장 큰 합과 그때의 n을 구하는 프로그램을 작성하시오

```c
#include  <stdio.h>
int main() 
{ 
int i,sum; 
for(i=1,sum=0;sum<10000;i++) 
  { 
  sum=sum+i; 
  if(sum+i>10000) 
      break; 
  } 
printf("1부터 n까지의 합중에서 10000을 넘지 않은 가장 큰 합은 %d, 그때의 n은 %d",sum,--i); 
}
```

간단한 문제였는데 어이없게 헤매던 문제, for문으로 굴리고 빠져나오는 거는 if break로 하면 되는데

계속해서 한 계단 큰 수가 나왔다. if(sum+i>10000) 이렇게 줘야 하는 거를 if(sum>10000) 이렇게 해둔 게 원인이었다.;; 

### 1부터 n까지의 공중에서 10000을 넘지 않는 가장 큰 곱과 그때의 n을 구하는 프로그램을 작성하시오

```c
#include  <stdio.h>
int main() 
{ 
int i,j=1; 
for(i=1;j<=10000;i++) 
  { 
  j=j*i; 
  if(j*i>10000) 
  break; 
  } 
printf("1부터 n까지의 곱중에서 10000을 넘지 않는 가장 큰 곱은 %d, 그때의 n은 %d",j,i); 
}
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969662-2d35f984-1460-421d-bf0b-5dc4708b3229.png)

뭐 위에 문제랑 마찬가지다. +랑 x차이일뿐

###  다음 조건을 만족하면서 정수를 입력받아 32비트의 비트 정보를 모두 출력하는 프로그램을 작성하시오 (입력받은 정수가 0이면 종료하고 0이 아니면 계속 수행)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969724-2b50364a-d080-4a92-9d0d-e7dd48c4412d.png)

```c
#include  <stdio.h>
int main(void) 
{ 
    int n; 
    int i; 
    printf("정수 입력:"); 
    scanf_s("%d", &n); 
  if(n==0) 
  printf("종료하겠습니다."); 
  else{ 
    for (i = 31; i >= 0; i--) 
    { 
        if (n & (1 << i)) 
        { 
            putchar('1'); 
        } 
        else 
        { 
            putchar('0'); 
        } 
    } 
    putchar('\n'); 
} 
}
```

나름 고민했었는데 결국 못풀고 답지를 봤다. 나는 pow문을 이용해서 2^32를 구하고 이거를 >>1 을 이용해서 한 칸씩 이동하고 &연산자를 사용해서 비트를 구하고 if연산자를 이용해서 0과 1일 때로 나누려고 했는데 그런 멍청한 짓 하지 말고

1일 경우 2진수에서 첫자리를 차지하고 있을 것이다.

그러면 그거를 비트 연산으로 31칸 옆으로 보내면 32비트에서 맨 끝자리가 1이고 나머지가 0인 수가 나올 것이고 이거를 for문을 이용해서 하나하나 연산하면 된다. putchar을 하던 printf를 하던 마음대로 하고

if (n & (1 << i)) 라고 하니 생각보다 간단하게 문제가 풀렸다...

(입력받은 정수가 0이면 종료하고 0이 아니면 계속 수행)에 대해서는 if else를 이중으로 걸어버렸다.

 

뭐 아니면 if로 경우를 나누지 말고 printf("%d",n & (1 << i)) 바로 걸어 버려도 상관없을 것이다.

### 나는 100원짜리와 50원짜리 동전을 합하여 15개 가지고 있다,친구는 100원짜리 동전은 나의 반, 50원짜리 동전은 나의 3배를 가지고 있는데 돈의 합계는 같다.계산하여 아래와 같이 출력한다.내가 가진 100원 개수:x, 50원 개수:y 금액 :xx친구 가진 100원 개수:x, 50원 개수:y 금액 :xx

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969822-f56418b1-d0b8-44e7-9a44-f3107dc7394e.png)

```c
#include  <stdio.h>
int main() 
{ 
int i,j,a,b; 
for(i=1,j=14;;i++,j--) 
{ 
  a=i*100+j*50; 
  b=i*100/2+j*50*3; 
  if(a==b) 
	break; 
} 
printf("내가 가진100원 개수:%2d, 50원의 개수 :%2d 금액:%d\n",i,j,a); 
printf("친구 가진100원 개수:%2d, 50원의 개수 :%2d 금액:%d\n",i/2,j*3,b); 
}
```

이 문제는 어떻게 100원과 50원의 개수를 알 수 있지? 라고 생각했는데 그냥 1부터 for문 돌려버리고 금액이 같을때 빠져 나온다고 생각하니 쉽게 구할 수가 있었다. 생각보다 생각하게 만든 문제

변수는 동전의 개수 i,j 친구와 나의 금액을 지정해줄 a,b

if를 쓰지말고 a!=b라고 할까하다가. 그러면 계산이 끝나야하는데 한번더 계산된 값이 나올거 같다. 

### 다음과 같은 함수가 주어졌을 때 x의 값을 -3부터 3까지의 해 구하기(1씩 증가하면서) (for와 if 이용)f(x) = x^3 - 9x +2 (x<=0)f(x) =7x + 2 (x>0)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209969870-1717152b-8d8d-47e4-9a95-755edf10fb07.png)

```c
#include  <stdio.h>
int main() 
{ 
int x=-3,y; 
for(x=-3;x<=3;x++){ 
  if(x<=0) 
    { 
    y=(x*x*x-9*x+2); 
    printf("x:%2d, y = %2d\n",x,y); 
    } 
  else 
    { 
    y=(7*x+2); 
    printf("x:%2d, y = %2d\n",x,y); 
    } 
  } 
}
```

2가지 경우의수와 반복해서 올라가는 숫자 때문에 조금 난해하게 보일 수 있는데 나름 간단한 문제

x*x*x가 지저분해 보인다면 pow(x,3)을 사용해도 될 것이다.

 

더 깔끔하게 하는 방법이 있는데 3항연산자 조건문을 이용해서 (x<=0)? 식1 : 식2 이렇게 처리하면 if,else없이 바로 가능하기도 하다.

### 다음을 참고로 표준 입력으로 받은 0에서 360도의 각도가 있는 평면의 사분면을 출력하는 프로그램을 작성하시오ex) 50도는 0도는 양의 x축 1사분면 출력 90도는 '양의 y축' 180 도는 음의 x축, 270도는 음의 y축으로 출력

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209970068-74204189-c9ca-4b54-9950-ab689616faa3.png)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209970081-5fa90890-9586-43c0-baf9-b8c7f313db53.png)

```c
#include  <stdio.h>
int main() 
{ 
int i,j; 
printf("각도 입력 :"); 
scanf("%d",&i); 
if(i==0) 
printf("%d도는 양의x축",i); 
else if(i==90) 
printf("%d도는 양의y축",i); 
else if(i==180) 
printf("%d도는 음의x축",i); 
else if(i==270) 
printf("%d도는 음의y축",i); 
else if(i>0&&i<90) 
printf("%d도는 1사분면",i); 
else if(i>90&&i<180) 
printf("%d도는 2사분면",i); 
else if(i>180&&i<270) 
printf("%d도는 3사분면",i); 
else if(i>270&&i<360) 
printf("%d도는 4사분면",i); 
else if(i<0||i>360) 
printf("잘못입력하셨습니다."); 
}
```

그냥 평범하게 scanf로 받고 일일이 모든 경우의 수를 지정해주면 된다.

### 다음을 참고로 표준 입력으로 받은 두 실수의 연산을 수행하는 프로그램을 작성하세요

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209970124-b52044e2-52ae-4124-93af-b90d10178d58.jpg)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209970134-cbd90e98-5047-4bf5-9465-44a8a0d3b55b.jpg)

```c
#include  <stdio.h>
int main() 
{ 
double a,b; 
int k;  
printf("두 실수 입력 : "); 
scanf("%lf %lf",&a,&b); 
printf("연산 종류 번호 선택 1<+>, 2<->, 3<*>, 4</> : "); 
scanf("%d",&k); 
if(k==1) 
printf("%.3lf + %.3lf = %.3lf",a,b,a+b); 
else if(k==2) 
printf("%.3lf - %.3lf = %.3lf",a,b,a-b); 
else if(k==3) 
printf("%.3lf * %.3lf = %.3lf",a,b,a*b); 
else if(k==4) 
printf("%.3lf / %.3lf = %.3lf",a,b,a/b); 
else 
printf("잘못입력하셨습니다."); 
}
```

보기좋으라고 그냥 3자리로 끊어 봤는데, 이것도 if와 scanf만 사용 할줄 알면 쉽게 푼다.

나는 이상하게 switch문이 익숙하지 않아서 사용을 안했는데 그거를 사용하는게 편할거다.

switch(해당하는 변수)

{

case a : ~ case b: .... 등등등 하면 된다. 

}

### 다음을 참고로 표준입력으로 받은 신장(키)과 몸무게를 이용하여 비만 정도를 출력하는 프로그램을 작성하시오BMI 계산법 :  BMI = 몸무게 / (신장 * 신장)BMI지수가 18.5미만: 저체중, 18.5~23은 정상, 23~25은 과체중, 25~30은 비만, 30~35은 고도비만 35이상은 초고도비만

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/209970187-b8bfbc37-e2ee-4811-8e97-35113d082ef8.png)

```c
#include  <stdio.h>
int main() 
{ 
double m,bmi; 
int kg; 
printf("키(m), 몸무게(kg)입력 : "); 
scanf("%lf %d",&m,&kg); 
bmi=kg/(m*m); 
printf("키 : %.2f, 몸무게 %d의 bmi:%f는 ",m,kg,bmi); 
if(bmi<=18.5) 
printf("저체중"); 
else if(bmi>=18.5&&bmi<=23) 
printf("정상체중"); 
else if(bmi>=23&&bmi<=25) 
printf("과체중"); 
else if(bmi>=25&&bmi<=30) 
printf("비만"); 
else if(bmi>=30&&bmi<=35) 
printf("고도비만"); 
else if(bmi>=35) 
printf("초고도비만"); 
}
```

이것도 뭐 실수이니 정수로 주는 실수를 하면 안되는거 말고는 크게 어려울 것은 없는거 같다.

지금보니 조건을 조금 잘못 주기는 했는데 if문이라는게 거르고 넘어가는거다보니 문제는 없을 것이다.

### 다음을 참고로 표준입력으로 받은 종합소득 금액에 따른 세율을 출력하는 프로그램을작성1200만원 이하 6% // 4600만원 이하 15% // 8800만원 이하 24% // 3억원 이하 35% // 3억원 초과 38%

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071416-dcbbfec7-33f9-4dbc-9f2f-3aa13e052366.png)

```c
#include  <stdio.h>
int main() 
{ 
int i; 
printf("소득을 입력하세요(단위:만원 1200만원인경우 1200) :"); 
scanf("%d",&i); 
printf("%d만원의 세율은 ",i); 
if(i<=1200) 
printf("6%%"); 
else if(i<=4600) 
printf("15%%"); 
else if(i<=8800) 
printf("24%%"); 
else if(i<=30000) 
printf("35%%"); 
else if(i>=30000) 
printf("38%%"); 
}
```
이거는 뭐... 설명할 것도 없을거 같은데

### 문자 하나와 온도를 실수형으로 입력 받아, 문자가 F나 f이면 입력 받은 값을 화씨로 간주하여 섭씨로 바꾸고, 입력받은 문자가 C나 c면 입력받은 값을 섭씨로 간주하여 화씨로 바꾸어 결과를 출력하는 프로그램을 작성하세요F = (9.0/5.0)*C + 32C = (5.0/9.0)*(F-32)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071453-0ff4063f-5ec1-4bd7-9bfd-43196bdc606c.png)

```c
#include  <stdio.h>
int main() 
{ 
double k; 
int i; 
char j; 
printf("온도입력(예 53F, 25C):"); 
scanf("%d%c",&i,&j); 
if(j=='c'||j=='C') 
  { 
  k=((9.0/5.0)*i+32); 
  printf("%d%c = %.2fF",i,j,k); 
  } 
else if(j=='f'||j=='F') 
  { 
  k=((5.0/9.0)*(i-32)); 
  printf("%d%c = %.2fC",i,j,k); 
  } 
}
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071478-2eee20ef-7bed-4661-8e01-4645fa341ce7.png)

처음부터 C와 F인거를 알지 못하고 입력받는걸로 구분하라고해서 당황스러웠는데 상당하게 간단하게 풀렸다. 뒤에 F,C를 scanf로 마찬가지로 받아주고 if문으로 C인지 F인지 구분해주면 됬다. 그런데 여기서는 소문자와 대문자를 구분하니 if(j=='c'||j=='C'),  else if(j=='f'||j=='F')으로 해주고, 뭐 아니면 잘못입력했다고 해주면 되니.

처음에는 스페이스바없이 그냥 바로 입력이 되나 했는데, 정수형과 문자형은 확실히 다르니

물론 if문 대신 switch문으로 해도 된다.



### 각자리수의 짝수 홀수 판단해보기정숫값 하나를 입력받는다. (예를 들어 352라고 하자)각 자릿수의 숫자의 홀수인지 짝수인지 판단하여 100의 자리 3은 홀수, 10의 자리 5는 홀수, 1의 자리 2는 짝수’라고 출력한다.나눗셈 연산자(/)와 나머지 값을 계산하는 연산자(%)를 사용한다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071496-62c04bd4-3ee4-4a88-a67a-c0b2377a8c1e.png)

필요한 변수는? : num, digit_1, digit_10, digit_100

```c
#include  <stdio.h>
int main() 
{ 
int num, digit_1, digit_10, digit_100; 
printf("정수값 하나 입력하기: "); 
scanf("%d",&num); 
digit_100=num/100; 
digit_10=num/10; 
digit_1=num%10; 
if(digit_100%2) 
printf("\n100의 자리는 홀수\n"); 
else 
printf("\n100의 자리는 짝수\n"); 
if(digit_10%2) 
printf("\n10의 자리는 홀수\n"); 
else 
printf("\n10의 자리는 짝수\n"); 
if(digit_1%2) 
printf("\n1의 자리는 홀수\n"); 
else 
printf("\n1의 자리는 짝수\n"); 
}
```

일단 3자리라고 가정을 하고 풀어 봤는데, 3자리가 넘어가는 경우에 1000으로 나누었을때 몫이 1이상이면 빠져나온다거나, 아니면 num>=1000을 해도 될것이다. 여기서는 변수를 1,10,100으로 주어져있어서 간단하게 해보았다.

위에 do-while문을 이용해서 임의의 자리수를 입력해도 각 자리수를 알 수 있지 않을까 했는데 

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071519-f829cde3-05cf-48b2-87ca-683cfd1b2cbf.png)

```c
#include  <stdio.h>
int main() 
{ 
int num, digit_1, digit_10=1; 
printf("정수값 하나 입력하기: "); 
scanf("%d",&num); 
digit_1; 
do{ 
digit_1=num%10; 
if(digit_1%2) 
printf("%d의 자리는 홀수입니다.\n",digit_10); 
else 
printf("%d의 자리는 짝수입니다.\n",digit_10); 
num=num/10;  
digit_10=digit_10*10; 
}while(num); 
}
```

가능은하다. 반대로는 어떻게 해야할지 모르겠다는게 함정이지만

### 놀이공원 입장료 계산하기

12세이하 또는 65세 이상은 25,000 성인은 35,000, 야간입장(17시 이후)에는 인당 1만원

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071565-a30d80ec-8505-4633-9c8d-d448d7ea516b.png)

```c
#include  <stdio.h>
int main() 
{ 
int age,time,count,fee,final_fee; 
printf("나이 입력 : "); 
scanf("%d",&age); 
printf("인원수 입력 : "); 
scanf("%d",&count); 
printf("입장시간(24시 기준) : "); 
scanf("%d",&time); 
if(time>=17) 
{ 
fee=10000; 
printf("인당 입장료 : %d\n",fee); 
final_fee=fee*count; 
printf("총 입장료 : %d",final_fee); 
} 
else if(age>=65||age<=12) 
{ 
fee=25000; 
printf("인당 입장료 : %d\n",fee); 
final_fee=fee*count; 
printf("총 입장료 : %d",final_fee); 
} 
else 
{ 
fee=35000; 
printf("인당 입장료 : %d\n",fee); 
final_fee=fee*count; 
printf("총 입장료 : %d",final_fee); 
} 
}
```

변수는 3가지 나이, 인원, 입장시간 거기에다가 야간입장의 경우 나이와 상관없이 할인이 되니 먼저 적용해주고 나머지 각각 물어보는 식으로 하면 쉽게 처리가 된다.

 

자주실수하는 오류 오타는 말할 것도 없고,

정수출력-실수출력 잘못지정,(f로 입력받아야 하는데 d로 지정해주고 값이 안나온다거나)

scnaf("%d",&i) 에서 "여기"에 쉼표 같은 엉뚱한거 집어 넣거나,

&i 처럼 &라고 주소연산자를 지정해줘야 하는데 i만 달랑 쓴다거나 &대신에 %집어 넣기

for, if문 뒤에 세미콜론 ; 붙여버리기


### 프로그램 사용자로부터 양의 정수를 하나 입력 받아서 ,그 수만큼 "Hello world!"를 출력하는 프로그램을 작성해보자.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071597-f0ce465d-3ddf-4359-8cef-e26e90ea80a2.png)

시험시간이 3시간정도 남았는데 그냥 문제 보이는대로 닥치는데로 풀어볼려고 한다.

```c
int main() 
{ 
int i,j=0; 
printf("양의 정수를 입력하세요 : "); 
scanf("%d",&i); 
while(i>j) 
{ 
  j++; 
  printf("Hello wolrd! %d\n",j); 
} 
}
```


### 프로그램 사용자로부터 양의 정수를 하나 입력 받은 다음, 그 수만큼 3의 배수를 출력하는 프로그램을 작성해 보자, 예를 들어서 5를 입력 받았다면 3 6 9 12 15를 출력해야 한다.


![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071629-9a2e3e6f-1938-4ffe-bca7-299ffd67399a.png)

```c
int main() 
{ 
int i,j=0; 
printf("양의 정수를 입력하세요 :"); 
scanf("%d",&i); 
while(i>j) 
{ 
  j++; 
  printf("%d ",j*3); 
} 
}
```

### 사용자로부터 계속해서 정수를 입력 받는다. 그리고 그 값을 계쏙해서 더해 나간다. 이러한 작업은 사용자가 0을 입력할때까지 계속되어야 하며, 0이 입력되면 입력된 모든 정수의 합을 출력하고 프로그램을 종료 시킨다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071666-ab97a7d1-353a-4dc2-9f71-426a24d0fa37.png)

```c
int main() 
{ 
int i,sum=0; 
while(1) 
{ 
  printf("숫자를 입력하세요 :"); 
  scanf("%d",&i); 
  sum=sum+i; 
  if(i==0) 
  break; 
} 
printf("총합은 %d 입니다.",sum); 
}
```

### 입력받은 숫자에 해당하는 구구단을 출력하되 역순으로 출력하는 프로그램을 작성해보자

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071698-c15f4ac3-e0d6-4c48-a7cd-bedfeef82b29.png)

```c
int main() 
{ 
int i,j=9; 
printf("구구단할 숫자를 입력하세요: "); 
scanf("%d",&i); 
while(j>0) 
{ 
  printf("%d x %d = %d\n",i,j,i*j); 
  j--; 
} 
}
```

### 입력받은 정수의 평균을 출력하는 프로그램을 작성하되 다음 두 가지 조건을 만족시켜야 한다.몇개의 정수를 입력할지 사용자에게 묻는다, 그리고 그 수만큼 입력받는다.

평균 값은 소수점 이하 까지 계산해서 출력한다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210071737-25fd9b82-ce5c-4dc1-9f33-a16723ca5f7e.png)

```c
int main() 
{ 
int num,j=1; 
double avg,k,i; 
printf("몇개의 정수 : "); 
scanf("%lf",&i); 
while(i>=j) 
{ 
  printf("정수를 입력하세요"); 
  scanf("%d",&num); 
  k=k+num; 
  j++; 
 } 
printf("평균값은 %g입니다.",k/i); 
}
```

끝!