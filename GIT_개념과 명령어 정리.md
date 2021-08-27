#  GIT_ Branch & Conflict

```bash
git branch apple
# -> apple 이라는 branch 만듬

git branch
# -> branch 목록 보기

git log --all --graph --oneline
# -> log 보는 명령어인데, 그래프형태 및 한줄로 모든 것을 보여준다는 의미

git checkout apple
git checkout master
# -> branch 이동할 수 있다.

git commit --amend
# -> commit msg 바꿀 수 있다.
```



## 1. 브랜치의 사용법

> branch 생성

`git branch 브랜치명`  



> branch 이동(전환)

`git checkout 브랜치명` : checkout 하게 되면 해당 branch 의 마지막 commit 상태로 돌아가게 된다.



## 2. 브랜치 병합(merge)

![image-20210820112513456](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210820112513456.png)



**아이디어**

- `a`에서 작업 하던 것을 `m` 에게도 있었으면 좋겠다 ! 

- a + m => `merge commit` 을 만든다 !



#### master, o2 branch 합치기(master 가 기준이 된다.)_file이름이 다른것들 병합

1. master branch로 이동

`git checkout master`

2. merge o2 한다

`git merge o2` : 이후에 commit msg 입력 

![image-20210820114251731](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210820114251731.png)



#### master, o2 branch 합치기(master 가 기준이 된다.)_file이름이 같은것 병합



>  `master branch` 의 work.txt

```
title

master content



title

content
```



>  `o2 branch`의 work.txt

```
title

content



title

o2 content
```



>  `$ git merge o2 (master)`

```
title

master content


title

o2 content
```



![image-20210820130617231](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210820130617231.png)



#### master, o3 branch 합치기(master 가 기준이 된다.)_same file, same part

> master의 work1.txt

```bash
# Title
content
master
# Title
content
```



> o3의 work1.txt

```bash
# Title
content
o3
# Title
content
```



`git checkout master` -> `git merge o3` 의 결과는 다음과 같다.

![image-20210823101931740](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210823101931740.png)

![image-20210823102117488](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210823102117488.png)



work1.txt 에 들어가게 되면 각 branch(master, o3) 가 한 부분에 대해 나와있으므로 내가 알아서 고치고 

`git add .` 입력을 하게 되면 `내가 이 부분 수정 완료했다` 라는 의미를 전달할 수 있다. 이 후 `commit`까지 완료하게 되면 merge 가 마무리 된다.



## 3 way merge

| here | base | there |
| :--: | :--: | :---: |
|  A   |  A   |   A   |
|  H   |  B   |   B   |
|  C   |  C   |   T   |
|  H   |  D   |   T   |

에서 **here** 과 **there**을 합치려고 할 때 3 way merge 의 이론적인 내용이 들어간다.

> base 와 비교해서 변화를 중심으로 merge 한다.

| here | there | merge(result) |
| :--: | :---: | :-----------: |
|  A   |   A   |       A       |
|  H   |   B   |       H       |
|  C   |   T   |       T       |
|  H   |   T   |      ??       |

 

# GIT Back Up

![image-20210823172144502](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210823172144502.png)

### 1. 지역저장소 -> 원격저장소와 연결

```bash
# HTTPS 이용하여 원격 저장소의 repository와 연결

git remote add origin https://github.com/fifa920/my-repo.git

# 원격 저장소에 잘 연결됐는지 확인
git remote -v

# 그 결과
origin  https://github.com/fifa920/my-repo.git (fetch)
origin  https://github.com/fifa920/my-repo.git (push)

```



### 2. Push

`git push origin master` or `git push` 를 통해 remote repository 에 업로드 한다.



### 3. 복제(Clone)

-> remote repo 에서 다른 local repo 로 해당 내용을 clone 할 수 있다.

HTTPS 주소를 이용하여 Clone 한다.

```bash
git clone https://gitbub.com/fifa920/my-repo.git
```



### 4. Pull

```bash
# pull 을 먼저 땡겨온다음 작업후 push 하는 순서로 작업하자

$ git pull

$ git add .
$ git commit -m 'msg'
$ git push 
```





# GIT 협업

`a`, `b` 라는 두 명의 사람이 작업을 하는 연습을 해보자.

```bash
$ git init a
$ cd a

$ nano work.txt
# 내용을 입력
$ git add .
$ git commit -m 'work 1'

# github에 가서 git4-collaboration-cli 라는 new repository 를 생성
# remote repository를 등록한다.
$ git remote add origin https://github.com/fifa920/git4-collaboration-cli.git

# -u 옵션을 통해 local master 와 remote master 을 연결시켜준다.(한번만 진행하면 됨)
$ git push -u origin master

# b라는 사람(여기서는 repository) 이 a 가 작업한 remote repository 를 clone 받는다.

$ cd .. # a repo 에서 나와서

    # 방법 1
    $ git init b
    $ cd b

    # 방법 2
    $ git clone https://github.com/fifa920/git4-collaboration-cli.git



```

![image-20210824095350026](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210824095350026.png)

- push 가 잘 된것을 확인할 수 있다.



> 협업할 사람을 초대해야 push 가 가능하다.

![image-20210824095717009](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210824095717009.png)

- github에 들어가 **Settings** 에 들어가 **Manage access** 클릭하여 `Invite a collaborator` 를 들어간 후 github ID를 입력하여 초대 메일을 보낸다.



> pull = fetch + merge FETCH_HEAD

- FETCH_HEAD 는 원격 저장소의 가장 최신의 버전을 가리키고 있고 그것을 의미한다.

- local 의 master 와 remote의 master(HEAD)를 일치시킨다.

