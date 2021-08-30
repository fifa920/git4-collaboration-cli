# Virtual Box 를 활용한 Ubuntu 설치

> ubuntu iso 파일 다운

https://ubuntu.com/download/desktop/thank-you?version=20.04.2.0&architecture=amd64

에 들어가서 다운로드 한다.



![image-20210805095828418](C:\Users\155443\Desktop\Me\Infra\server_우상호수석님\image-20210805095828418.png)

*>>* VirtualBox 에서 `새로만들기` 클릭 후 리눅스와 우분투를 설치한다.





![image-20210805100311233](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210805100311233.png)

*>>* 설정에 들어가 ISO 파일을 추가하여 설치완료



![image-20210805100753961](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210805100753961.png)

*>>*

![image-20210805101407693](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210805101407693.png)



> GIT 설치

```bash
$ sudo apt-get update
$ sudo apt-get install git

# SSH Server 관리를 위한 설치
$ sudo apt-get install openssh-server
$ sudo apt-get install openssh-client


```



> root PW 재설정

```bash
$ sudo passwd root
# 현재 계정 암호 입력
# 새로운 root 암호 입력
# 새로운 root 암호 재입력
```



> gitolite, git-repo install

```bash
$ adduser gitolite
$ adduser git-repo

# 비밀번호 설정하고 이름 등의 정보는 그냥 엔터를 눌러 스킵한다.
```

생성된 gitolite 계정은 사용자가 저장소에 접근할 때 사용하게 됩니다.

git-repo 계정은 저장소를 관리하는데 사용됩니다.



> SSH Key 생성

`ssh-keygen` 입력 후 enter > SSH Key 생성



![image-20210809152022118](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210809152022118.png)



기본 경로는 home/gitolite/.ssh/ 이고 해당 경로 아래에 id_rsa라는 파일이 생성

간단하게 ssh 키가 생성

키는 public과 private 키가 한 쌍으로 생성



> 생성된 public 키를 git-repo로 전달

`scp /home/gitolite/.ssh/id_rsa.pub git-repo@localhost:/home/git-repo/gitolite.pub`

즉, 저장소(git-repo)를 관리하는 계정으로 gitolite를 등록했기 때문에 이제부터 저장소에 관한 모든 동작은 "gitolite"에서 진행한다.



> git-repo 로 다시 넘어가 gitolite에서 생성하고 git-repo에 전달한 public 키를 등록

```bash
$ su - git-repo
$ ./gitolite/src/gitolite setup -pk ./gitolite.pub
```



> clone 명령어로 gitolite-admin 저장소 가져오기

```bash
$ su - gitolite
$ git clone git-repo@localhost:gitolite-admin.git
```

![image-20210810094524899](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210810094524899.png)

정상적으로 설치가 완료되었고 gitolite-admin에서 사용자를 추가하고 권한을 부여



> email, name 등록

```bash
$ git config --global user.email "gitolite@mymail.com"
$ git config --global user.name "gitolite"
$ git config --list
```

![image-20210810094841641](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210810094841641.png)



`--list` 명령어를 통해 잘 등록된 것을 확인할 수 있다.



> 사용자 계정(user1) 생성, `testing.git` 을 clone하고 수정, push, clone 실행

```bash
$ sudo useradd user3
$ su - user3
$ git config --global user3.email "user3@myemail.com"
$ git config --global user3.name "user3"
$ git config --list
localhost:gitolite-admin.git
```

![image-20210813093955273](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210813093955273.png)



> 사용자의 SSH Key를 생성하고, public key를 gitolite에 등록

```bash
$ su - user3
$ ssh-keygen
$ scp ~/.ssh/id_rsa.pub gitolite@localhost:/home/gitolite/gitolite-admin/keydir/user3.pub
```

![image-20210813094033033](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210813094033033.png)

Enter file 위치 : 빈공간 enter

passphrase 비밀번호는 10-30 자리로 지정 : tlffur12!



```bash
$ su - gitolite
$ cd gitolite-admin
$ git add keydir/user3.pub
$ git commit -a -m "add user user3"
$ git push

// 사용자를 삭제하는 경우
$ git rm keydir/user3.pub
$ git commit -a -m "remove user3"
$ git push
```

생성된 SSH Key를 활용하여 public key 를 gitolite에 등록한다.


> user3 계정에서 clone, push

사용자(user3) 계정에서 `testing.git`를 `clone`한다.

![image-20210813094356861](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210813094356861.png)



![image-20210813100805897](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210813100805897.png)



`testing.git`가 비어있기 때문에 `warning: You appear to have cloned an empty repository.` 경고문이 발생한다.
"testing"폴더에서 새로운 파일을 생성하고, Git 저장소에 `push`한다.



> git clone 을 통해 제대로 push 됐는지 확인

![image-20210813101627974](C:\Users\155443\AppData\Roaming\Typora\typora-user-images\image-20210813101627974.png)

`user3` 계정의 다른 폴더에서 `testing.git`를 가져오고, `file.txt`가 존재하는지 확인한다. => `Good !`




