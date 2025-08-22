---
title: 배열 포인터(array pointer)와 포인터 배열(pointer array) 차이 정리
date:   2022-06-13 14:37:30 +0900
categories: [Languages, C]
tags: [c,coding,pointer,array]
---

C/C++에서는 메모리를 직접 제어하기 위해서 포인터를 사용합니다. 그러면서 포인터에서 파생되는 개념이 많은데 배열포인터/포인터배열도 마찬가지 입니다. 만약 본인이 C를 공부를하고 있다면 이것을 피할 수가 없으니 꼭 짚고 넘어갑시다.

### 1. 포인터 배열 pointer array
포인터 배열은 포인터들의 배열입니다. 즉 배열의 요소가 포인터 들로 이루어져 있습니다.

![img1 view](https://user-images.githubusercontent.com/85277660/210573682-797d206d-e849-45de-b3d9-a1804a2895e8.png)

배열은 자료형을 정의를 해주어야 합니다. 나는 어떤 것을 담을지 선언을 해야하는데 포인터 배열은 어떤 자료형의 포인터들이 가득 들어 있다는 것을 생각해볼 수 있습니다.

그렇다면 위 그림처럼 직접적으로 어떤 값이나 단어를 가지는것이 아니라 그 값이나 단어의 위치(주소)를 배열로 담고 있습니다.

```c
#include <stdio.h>
#include <stdlib.h>
#define _CRT_SECURE_NO_WARNINGS

int main(void){
	const char* arr[3];

	arr[0] = "AAA";
	arr[1] = "BBB";
	arr[2] = "CCC";
	
	for (int i = 0; i < 3 ; i++) {
		printf("arr[%d]이 가르키는 값은 : %s\n", i, arr[i]);
	}

	return 0;
}
```

![point](https://user-images.githubusercontent.com/85277660/210573755-bbf40c5b-651e-4dae-bd6a-0a7f7a814f88.png)

얼핏보면 그냥 배열에다가 문자열을 넣는게 아닌가 하지만 다시한번 생각하셔야 할게 있습니다. 배열의 이름은 주소입니다. char* 포인터 배열을 사용했으며 그냥 포인터가 아닌 배열을 사용했다면 한글자만 담을 수 있습니다. 하지만 포인터로 선언한 다음 각 문자열 주소에다가 어떤 것을 저장할지 지정을 해주게 되고 이를 바탕으로 불러오는 값입니다.

[동적할당과 정적할당 차이 정리 (malloc, free) 함수 사용법](https://jeong-daniel.github.io/posts/%EB%8F%99%EC%A0%81%ED%95%A0%EB%8B%B9%EA%B3%BC-%EC%A0%95%EC%A0%81%ED%95%A0%EB%8B%B9-%EC%B0%A8%EC%9D%B4-%EC%A0%95%EB%A6%AC-(malloc,-free)-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9%EB%B2%95/)

마찬가지로 const char* 대신 입력을 받아서 maloc로 동적할당을 하는것 역시 가능합니다.

```c
arr[i] = (char*)malloc(sizeof(char) * len);
```
그럴때는 길이를 입력받아서 동적할당을 하면 됩니다.


### 2. 배열 포인터 array pointer
배열을 가리키는 하나의 포인터입니다. 그러니까 특정 사이즈의 배열만 가리킬 수 있는 하나의 포인터가 됩니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210574535-15b38255-36a7-49d3-b9a7-b10961489072.jpg)

```c
#include <stdio.h>
#include <stdlib.h>
#define _CRT_SECURE_NO_WARNINGS

int main(void){
	char(*arr)[3];

	char temp_1[3] = "AAA";
	char temp_2[3] = "BBB";
	char temp_3[3] = "CCC";
	printf("temp_1[3]의 주소 : %p\n", temp_1);
	printf("temp_2[3]의 주소 : %p\n", temp_2);
	printf("temp_3[3]의 주소 : %p\n", temp_3);
	
	arr = &temp_1;
	printf("arr의 주소 : %p\t 문자열 : ", arr);
	for(int i=0; i< (int)sizeof(*arr); i++){
		printf("%c", (*arr)[i]);
	}
	printf("\n");

	arr = &temp_2;
	printf("arr의 주소 : %p\t 문자열 : ", arr);
	for (int i = 0; i < (int)sizeof(*arr); i++) {
		printf("%c", (*arr)[i]);
	}
	printf("\n");

	arr = &temp_3;
	printf("arr의 주소 : %p\t 문자열 : ", arr);
	for (int i = 0; i < (int)sizeof(*arr); i++) {
		printf("%c", (*arr)[i]);
	}
	printf("\n");

	return 0;
}
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210574583-aea4695b-b1a6-4ad8-8c69-cef5a35736b5.png)

한글 출력이 매끄럽지 않아서 좀 이상하게 보일지모르겠는데 주소값만 보시면 됩니다.

temp_1, temp_2, temp_3를 참조하면 arr주소도 똑같이 가르키는 것을 볼 수 있습니다.

그렇다면 char[5]를 참조하면요?

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210574617-d90c945a-759f-4799-b5ac-4866317a4c65.png)

temp_3를 5개로 해서 출력을 하면 에러는 뜨지 않지만 문자열이 CCCCC가 아니라 3개로 짤립니다.

컴파일러에 따라서 에러를 출력할 수도 있습니다.