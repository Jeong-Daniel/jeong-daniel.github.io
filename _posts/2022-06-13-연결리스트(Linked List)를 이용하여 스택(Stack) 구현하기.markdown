---
title: 연결리스트(Linked List)를 이용하여 스택(Stack) 구현하기
date:   2022-06-13 17:21:30 +0900
categories: [Languages, C]
tags: [c,coding,stack,list]
---
앞에서 스택과 연결리스트를 각각 구현해보았습니다. 이번에는 연결리스트를 이용해서 스택을 구현하고자 합니다. 인터페이스는 스택에서 이미 언급을 했으니 바로 코드로 넘어가겠습니다.

[C언어로 연결 리스트 Linked List 자료구조 만들기](https://jeong-daniel.github.io/posts/C%EC%96%B8%EC%96%B4%EB%A1%9C-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8-Linked-List-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%A7%8C%EB%93%A4%EA%B8%B0/)

[C언어로 스택 stack 자료구조 만들기](https://jeong-daniel.github.io/posts/C%EC%96%B8%EC%96%B4%EB%A1%9C-%EC%8A%A4%ED%83%9D-stack-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%A7%8C%EB%93%A4%EA%B8%B0/)

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210578667-225b53fb-26a8-4dcd-bf1c-216a3b9346b3.png)

연결리스트와 스택구조는 위 게시글을 참고해주시면 됩니다.

```c
Stack *mkStack() {
	Stack* s = (Stack*)malloc(sizeof(Stack));
	assert(s);
	*s = mkList();
	return s;
}
```
스택을 하나 할당하고 연결 리스트를 새로 만들어 여기에 스택으로 리턴합니다.

```c
void push(Stack* s, int item) {
	addFront(s, item);
}
```
push는 addFront를 사용합니다.

```c
int pop(Stack* s) {
	int data;
	if (isEmpty(s)) error("스택이 비어있습니다. ");
	data = delFront(s);
	return data;
}
```

pop는 연결 리스트의 delFront를 이용해서 이용합니다. 이때 isEmpty로 비어있는지도 확인합니다.

```c
int isEmpty(const Stack* s) {
	return isEmptyList(*s);
}

int isFull(const Stack* s) {
	return 0;
}
```
Empty는 연결 리스트가 비어있다면 리스트역시 비어있다는 의미이니 재활용합시다.

Full은 자유 저장 공간을 사용하니 컴퓨터의 모든 메모리를 사용하지 않는 이상 가득 차지는 않으니 0으로 리턴합시다.

 

자유저장 공간이 꽉 차는 경우를 검사하려면 Stack의 push의 함수, 즉 List의 addFront함수에서 검사하는 것이 바람직합니다.

```c
#include <stdio.h>
#include <stdlib.h> 
#include <assert.h>
#define _CRT_SECURE_NO_WARNINGS

struct _node {
	int data;
	struct _node* next;
};
typedef struct _node Node;
typedef Node* List;
//연결 리스트 타입 List정의 List는 Node*로 정의됨

void error(const char* msg) {
	printf("오류: %s ", msg);
	printf("프로그램을 종료합니다. \n");
}


List mkList() {
	List l = (List)NULL;
	return l;
}

void addFront(List* lp, int data) {
	Node* n = (Node*)malloc(sizeof(Node));
	n->data = data;
	n->next = *lp;
	*lp = n;
}

int delFront(List* lp) {
	Node* head = *lp;
	int data;
	if (isEmptyList(*lp))
		error("리스트가 비어있음");
	*lp = head->next;
	data = head->data;
	free(head);
	return data;
}

int isEmptyList(const List l) {
	return (l == NULL);
}

typedef List Stack;

Stack* mkStack();
void push(Stack* s, int item);
int pop(Stack* s);
int isEmpty(const Stack* s);
int isFull(const Stack* s);

Stack* mkStack() {
	Stack* s = (Stack*)malloc(sizeof(Stack));
	assert(s);
	*s = mkList();
	return s;
}

void push(Stack* s, int item) {
	addFront(s, item);
}

int pop(Stack* s) {
	int data;
	if (isEmpty(s)) error("스택이 비어있습니다. ");
	data = delFront(s);
	return data;
}

int isEmpty(const Stack* s) {
	return isEmptyList(*s);
}

int isFull(const Stack* s) {
	return 0;
}

int main() {
	int data;
	int n, i;
	Stack* s;

	printf("입력할 정수의 개수를 입력 : ");
	scanf("%d", &n);
	if (n <= 0)
		error("정수의 개수를 잘못 입력했습니다. ");

	s = mkStack();

	printf("%d개의 정수를 입력해주세요 ", n);
	for (i = 0; i < n; i++) {
		scanf("%d", &data);
		push(s, data);
	}

	printf("입력한 정수를 역순으로 출력합니다 : ");
	while (!isEmpty(s)) {
		data = pop(s);
		printf("%d ", data);
	}
	printf("\n");

	return 0;
}
```

![img1 result](https://user-images.githubusercontent.com/85277660/210578690-ffe09877-df62-4e23-ae24-c9c369219441.png)