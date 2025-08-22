---
title: Git 브랜치(Branch) 사용방법 정리
date:   2022-07-10 14:29:41 +0900
categories: [Tech, Git]
tags: [git, github]
---

혼자서 개발할때는 버전관리에 대해서 크게 고민할것이 없습니다. 아마 나눈다고 해봐야 테스트/배포용 정도로 나눠 볼 수 있겠습니다. 하지만 동시에 여러명이 어떤 것을 작업할때는 이야기가 달라집니다. 코드는 한가지지만 누군가는 버그를 픽스하고 누군가는 기능을 추가하고 싶어 할 것입니다.

이때 여러 개발자들이 동시에 다양한 작업을 할 수 있게 만드는 것이 바로 브랜치 입니다. Branch는 명사로는 나무가지, 지사/분점을 동사로는 갈라지다라는 의미를 가지고 있습니다.

![git branch](https://user-images.githubusercontent.com/85277660/210137460-317273cd-e4c2-49b1-b4ec-418da59c498f.png)

브랜치에도 종류가 있습니다.

 

### 통합 브랜치(Integration Branch) 출시 가능 브랜치
언제든지 배포 가능한 상태로 유지해야하는 브랜치입니다. 안정성이 최우선적으로 요구되는 브랜치로 처음 저장소를 만들었을때 자동으로 생성되는 master 브랜치를 통합 브랜치로 사용합니다.

 

### 토픽 브랜치(Topic Branch)  기능추가 버그 수정 같은 단위 작업을 위한 브랜치
토픽 브랜치에는 여러가지로 세부 구분 할 수 있습니다. 특정 작업이 완료되면 다시 통합 브랜치에 병합(merge)하는 방식으로 진행되며 토픽 브랜치는 다른 말로 피처 브랜치라고 하며 브랜치 종류는 아래와 같습니다.

 

develop 다음 버전을 개발 브랜치
기능 개발을 위한 브랜치들을 병합하기 위해 사용합니다. 평소에는 해당 브랜치를 기반으로 개발 진행하고 배포 가능한 상태라면 master 브랜치에 머징(합치기)를 합니다.

feature 기능 추가 브랜치
새로운 기능 개발 및 버그 수정이 필요할 때마다 develop 브랜치로부터 분기(branch)함
많은 경우 해당 브랜치는 공유할 필요는 없기에 로컬 저장소에서 관리


 

release 배포 전용 브랜치
develop branch 로부터 분기하며 출시 버전을 위한 브랜치로 한 팀이 해당 배포를 준비하는 동안 다른 팀은 다음 배포를 위한 기능 개발을 할 수 있음

hotfix 출시 버전에서 버그를 수정 브랜치
master branch 로부터 분기하여 필요한 부분만을 수정한 후 다시 병합

 

하지만 팀개발을 한다고 해서 모든 브랜치를 사용해야한다는 법칙은 없습니다. 팀 규모와 프로젝트에 따라서 브랜치를 유동적으로 관리하면 됩니다.


이어서 실습을 해보도록 하겠습니다.

```bash
$ mkdir git_branch
$ cd git_branch
$ git init
Initialized empty Git repository in C:/github/git_branch/.git/
```

새폴더를 만든 다음 git 저장소로 지정을 했습니다.

![git branch example](https://user-images.githubusercontent.com/85277660/210137475-e51bbbbf-5a35-483e-b8cb-55492f00d9c9.png)

그런 다음 my_branch.txt 라는 파일을 넣고 커밋을 합시다.

```bash
$ git add my_branch.txt
$ git commit -m "텍스트 파일을 추가했습니다."
[master (root-commit) cf94e32] 텍스트 파일을 추가했습니다.
 1 file changed, 1 insertion(+)
 create mode 100644 my_branch.txt
 ```

 자 그러면 자동으로 master에 커밋이 이루어졌습니다. 본격적으로 브랜치를 만들어 보겠습니다.

 

### 1. 브랜치 만들기

```bash
$ git branch <branchname>
```

브랜치를 생성하는 것은 branch 라는 명령어와 이름을 입력하면 됩니다.

![bit master](https://user-images.githubusercontent.com/85277660/210137499-4c5e62ed-b4dc-4f14-b4f0-fe7f6bacb864.png)

그리고 git branch를 입력하면 생성된 브랜치와 마스터가 보입니다. 현재 * 기호가 붙어 있는게 현재 선택된 브랜치입니다.

참고로 기본으로 나오는거는 master일때도 있고 main일때도 있습니다. 이게 마스터 말고도 하위 기능에 따라서 slave라고 표현을 하는데 이게 master-slave 주인과 노예라는 뜻이 인종차별이 연상되는 어감, 어휘적으로 문제가 있어서 main으로 통일하는 추세라고 합니다.

### 2. 브랜치 전환하기
이제 내가 어떤 작업 위해서 진행을 할지 브랜치를 명시적으로 지정을 해줘야 합니다. 이때 사용하는 명령어가 checkout입니다.

```bash
$ git checkout <branch>
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210137543-989983b4-b868-44fa-88cd-725d0477c18b.png)

이때 브랜치 생성과 체크아웃을 나누었는데 checkout 명령에 -b 옵션을 넣으면 브랜치 작성과 체크아웃을 한꺼번에 실행할 수 있습니다.

```bash
$ git checkout -b <branch>
```

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210137546-dd6b15d7-ad81-4ec3-a806-01c4a7ea4283.png)

브랜치 생성과 동시에 스위칭이 이루어졌습니다.

그리고 처음에 생성한 텍스트 파일의 내용을 추가하고 브랜치에다가 커밋을 진행합시다.

 

### 3. 브랜치 병합하기

브랜치 병합은 merge 명령어로 실행합니다. 이 명령어에 병합할 커밋 이름을 넣어 실행하면, 지정한 커밋 내용이 'HEAD'가 가리키고 있는 브랜치에 넣어집니다. (branch를 찍었을때 * 표시가된 곳) 'HEAD'는 현재 사용중인 브랜치에 위치하게 됩니다.

일단 first_branch에 변경된 내용을 master에 적용을 해줍시다. 우선 'master' 브랜치에 'HEAD'가 위치하게 만들어야 합니다. 앞서 checkout로 head를 변경한 것 처럼 하시면 됩니다.

```bash
$ git checkout master
Switched to branch 'master'

$ git merge first_branch
Updating cf94e32..a6c2b76
Fast-forward
 my_branch.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
```

이제 'master' 브랜치가 가리키는 커밋이 'first_branch'와 같은 위치로 이동했습니다. 이런 방식의 병합을 'fast-forward (빨리감기) 병합'이라고 합니다.


### 4. 브랜치 삭제하기
병합을 한다고 자동으로 브랜치가 사라지는 것은 아닙니다. 명시적으로 제거를 해주어야 합니다.

```bash
$ git branch -d <branchname>
```

브랜치를 제거하는 것은 branch 명령어에 -d 옵션을 지정하면 됩니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210137562-38fa20ee-427d-4828-9ab7-f6be91e1835b.png)

그리고 브랜치로 확인하면 master만 나옵니다.

 

### 5. 각 브랜치에서 작업하기
브랜치가 여러개이고 이를 동시에 처리하는 상황을 만들어 보겠습니다. first와 second 브랜치를 만들고 second로 전환해봅시다.

```bash
$ git branch
  first
  master
* second
```
second 브랜치에서 commit에 대한 설명을 추가해서 커밋을 해보겠습니다.

![img1 daumcdn](https://user-images.githubusercontent.com/85277660/210137572-b19265d1-f12f-4f90-b935-6c34070f5540.png)

그리고 다시 first 브랜치로 전환 한다음 여기서 또 내용을 수정해서 커밋을 해봅시다.

```bash
$ git checkout first
error: Your local changes to the following files would be overwritten by checkout:
        my_branch.txt
Please commit your changes or stash them before you switch branches.
Aborting
```
참고로 브랜치로 전환하기 전에 내용먼저 수정해버리면 위 처럼 에러가 납니다.

 

여기 까지 하셨으면 브랜치는 first와 second 그리고 각각 커밋된 텍스트의 내용은 다르게 됩니다.

 

### 6. merge 할 때 발생하는 충돌 해결
앞에서 두 브랜치에서 수정한 내용을 master로 통합해보겠습니다. 먼저 matser로 head를 이동한다음 first를 병합합니다.

```bash
$ git checkout master
Switched to branch 'master'

$ git merge first
Updating a6c2b76..43b58f6
Fast-forward
 my_branch.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
 ```

이렇게 하면 앞서 설명되었던 'fast-forward(빨리감기) 병합'이 실행됩니다. first 브랜치와 master이 합쳐졌습니다. 이어서 second를 병합합시다.

```bash
$ git merge second
Auto-merging my_branch.txt
CONFLICT (content): Merge conflict in my_branch.txt
Automatic merge failed; fix conflicts and then commit the result.
```

CONFLICT 충돌이 발생해서 자동으로 병합하는것에 실패했습니다. 

![text file](https://user-images.githubusercontent.com/85277660/210137608-bfdb0fae-0fa2-4c69-96bc-966eef1a72a3.png)

그리고 텍스트 파일을 열어 보시면 충돌 정보를 포함하여 내용을 통해 어떤 브랜치에서 어떤 부분이 충돌되었는지 확인할 수 있습니다. 충돌이 일어난 부분은 일일이 확인해서 수정해 주어야 합니다.

![text merge](https://user-images.githubusercontent.com/85277660/210137615-74e36e3c-058b-474d-b02d-59058233d446.png)

```bash
$ git add my_branch.txt
$ git commit -m "브랜치 내용을 병합"
[master c483c5c] 브랜치 내용을 병합
```

이렇게 진행한 병합은 충돌 부분을 수정했기 때문에 그 변화를 기록하는 병합 커밋이 새로 생성 되었습니다. 그리고 'master' 브랜치의 시작('HEAD')이 그 위치로 이동해 있는 것을 확인할 수 있습니다. 아래와 같은 방식을 'non fast-forward 병합'이라고 합니다.


다음번에는 rebase에 대해서 정리를 해보겠습니다.

rebase는?
같은 브랜치에 여러명이 작업한 경우 Merge branch 커밋이 생긴 경우 해소하기
현재 브랜치에 다른 브랜치에 다른 브랜치 커밋 로그를 붙이고 싶은 경우