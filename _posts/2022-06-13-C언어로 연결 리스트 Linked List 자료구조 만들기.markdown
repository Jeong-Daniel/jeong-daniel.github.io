---
title: C언어로 연결 리스트 Linked List 자료구조 만들기
date:   2022-06-13 16:57:30 +0900
categories: [Languages, C]
tags: [c,coding,stack]
---

[동적할당과 정적할당 차이 정리 (malloc, free) 함수 사용법](https://jeong-daniel.github.io/posts/%EB%8F%99%EC%A0%81%ED%95%A0%EB%8B%B9%EA%B3%BC-%EC%A0%95%EC%A0%81%ED%95%A0%EB%8B%B9-%EC%B0%A8%EC%9D%B4-%EC%A0%95%EB%A6%AC-(malloc,-free)-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9%EB%B2%95/)

앞에서 동적할당 사용하는 이유와 스택을 만들어 보았는데 가장 핵심적이라고 할 수 있는 연결리스트를 동적할당으로 구현해보겠습니다. 배열을 생성하는 것보다 연결리스트는 필요한 만큼 사용하니 메모리 관리게 용의하지만 문제는 개념을 정확히 모른다면 코드를 짜기가 어렵습니다.

![linked list](https://user-images.githubusercontent.com/85277660/210577727-3036bae1-760b-43f1-9930-cccee01c3cbf.png)

연결리스트는 데이터와 노드 그리고 처음과 끝을 알려줄 Head와 Tail이 있습니다.

연결리스트는 여러개의 "포인터"를 통해 연결해둔 리스트로 해더는 연결리스트의 맨 앞 항목을 가리키는 포인터, 맨 끝 항목의 포인터에는 NULL이 저장됩니다. 다음게 없다는게 마지막이라는 의미가 되겠지요

항목을 추가할 때 데이터를 저장하기 위한 공간과 포인터를 함께 할당합니다. 맨 앞에 항목을 추가하는 것은 매우 쉽지만 맨 끝에 항목을 추가하는 것은 어렵다는 것이 특징입니다.

마찬가지로 스택에서 처럼 인터페이스 부터 설계를 합시다.

```c
List mkList();
void addFront(List *lp, int data);
int delFront(List *lp);
int isEmptyList(const List l);
```

일단 첫번째로 리스트를 만드는 함수 그리고 addFront는 뒤에다가 리스트를 연결할 함수, delFront는 lp가 가리키는 연결 리스트의 맨 앞 항목을 제거하여 리턴, int isEmptyList(const List l); 연결 리스트 l이 비어있는지 검사

[C언어로 스택 stack 자료구조 만들기](https://jeong-daniel.github.io/posts/C%EC%96%B8%EC%96%B4%EB%A1%9C-%EC%8A%A4%ED%83%9D-stack-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%A7%8C%EB%93%A4%EA%B8%B0/)

스택 자료구조를 설명하면서 어떤것은 왜 포인터를 사용해서 인자를 전달받고 const를 이용해서 고정하는지 이야기 했으니 넘어가겠습니다.

```c
struct _node {
	int data;
	struct _node* next;
};
typedef struct _node Node;
typedef Node* List;
//연결 리스트 타입 List정의 List는 Node*로 정의됨

List mkList();
void addFront(List* lp, int data);
int delFront(List* lp);
int isEmptyList(const List I);
void error(const char* msg);

void error(const char* msg) {
	printf("오류: %s ", msg);
	printf("프로그램을 종료합니다. \n");
}
```
node 구조체와 인터페이스를 선언합니다. 구조체에서 자신의 구조체의 next를 부르는데 이를 자기참조 구조체라고 합니다.

```c
List mkList() {
	List l = (List)NULL;
	return l;
}
```
스택을 처음 생성할때 top을 0으로 한 것처럼 리스트를 처음 생성할때 해당 리스트는 아무 노드가 없기에 NULL로 초기화된 해더를 리턴합니다. (List)NULL에서 List는 포인터로 선언을 했습니다. 그러니까 포인터가 가르키는 것은 NULL임을 알려주는것입니다.

```c
void addFront(List* lp, int data) {
	Node* n = (Node*)malloc(sizeof(Node));
	n->data = data;
	n->next = *lp;
	*lp = n;
}
```
노드를 하나 추가하기 위해서는 두가지가 필요합니다. 어떤 리스트에 연결할 것인가, 추가할 데이터입니다.

첫번째는 노드를 새로 정적할당을 합니다. 그리고 새로 생성할 노드에 data를 지정을 하고 노드 앞에다가 붙이는 것이기 때문에 새로 생성한 노드의 다음은 기존에 추가할 노드입니다. 즉 링크를 연결하는 과정입니다.

다시 설명을 하자면 *lp는 헤더를 가르키는 리스트 포인트입니다 이를 새로 생성한 노드로 갱신합니다.

```c
int delFront(List* lp) {
	Node* head = *lp;
	int data;
	if (isEmptyList(*lp))
		error("리스트가 비어있음");
	*lp = head->next;
	data = head -> data;
	free(head);
	return data;
}
```
delFront는 일단 제거할 리스트의 포인터를 받습니다. 그리고 해당 포인터를 받아서 head를 만들어 줍니다.

리스트가 비어있는지 확인한 다음 head의 next 값을 받게 됩니다. 그리고 값을 받게 된다면 head를 할당 해제를 하게되고

lp는 자기 머리(head)의 다음 next를 가리키게된 값부터 시작을 합니다.

```c
int isEmptyList(const List l) {
	return (l == NULL);
}
```
스택과는 달리 리스트 길이의 제한은 메모리 공간 만큼입니다. 다만 제거하기 위해서는 비어있는지만 확인하면 되고 이는 리스트가 NULL인지만 체크하면 됩니다.