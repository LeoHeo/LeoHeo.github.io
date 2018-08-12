---
layout: post
title: 개발할때 자주 쓰는 것들에 대한 코드조각 모음
category: Java
---

최근 이직을 하게 되면서 쓰는 언어부터 해서 여러가지가 바뀌었다.

그래서 ~삽질을 몇번하다가~ 기록을 해야지 나중에 안 까먹겠구나 하는 마음에 코드조각 모음을 기록 한다.

이런 개발할때 자주쓰는것들은 평소에 [bookmark](https://github.com/LeoHeo/bookmark)에 저장해놓고 필요할때마다 가져다가 쓰는 형태이다.


## Two github Account
- 2개 이상의 github ssh를 관리 하기 위해서는 몇가지 설정 작업이 필요하다.
- {username} 은 넣고싶은 variable이다.

### ssh 키를 만든다. 

```
$ ssh-keygen -t rsa -C "username@gmail.com" # 새롭게 등록할 계정
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/{username}/.ssh/id_rsa): /Users/{username}/.ssh/id_rsa_{username} # 기존경로에 덮어쓰지 않게 주의한다.
```

### [github에서 ssh키 등록](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)하고ssh config를 만든다.

```
$ touch ~/.ssh/config
$ vi ~/.ssh/config
```

`config` 파일에 아래 내용을 붙여넣는다.

```
# Default account
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa
# another account
Host github.com-{username}
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_{username}
```

### git clone를 할때 주소에 username을 추가한다.

- 기본적으로 clone 할때는 git@github.com:LeoHeo/bookmark.git 한다.
- 2번째 계정으로 clone을 할때는 아래같이 해줘야 한다.
- git@`github.com-{username}`:LeoHeo/bookmark.git

```
$ git clone git@github.com-{username}:LeoHeo/bookmark.git
```

---

## 홀로 실행 가능한 JAR 만들기

- spring boot로 `./graldew bootJar`를 해서 jar 파일을 만들게 되었을때 실행을 아래처럼 한다.

```
$ java -jar build/libs/demo-0.0.1-SNAPSHOT.jar
```

- 근데 이렇게 하지 않고 [executable jar 만들기](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html/#packaging-executable-configuring-launch-script) 를 참고해서
build.gradle에 아래와 같이 넣어주면 홀로 실행 가능한 jar를 만들 수 있다.


```
bootJar {
  launchScript()
}
```

- 이렇게 추가를 하고 jar를 만들면 아래처럼 실행이 가능하다.

```
$ build/libs/demo-0.0.1-SNAPSHOT.jar
```


이 방법은 문서에 아래와 같이 나와 있다.
> On Unix-like platforms, this launch script allows the archive to be run directly like any other executable or to be installed as a service.

---

## mariaDB Docker-compose로 띄우기

### [docker for mac](https://www.docker.com/docker-mac) 설치

- docker for mac 설치 후 실행
- `$ docker --version` 으로 설치된거 확인

### docker-compose로 maria db 설치
- `$ touch docker-compose.yml` 생성

```yml
version: '3.1'

services:

db:
image: mariadb
restart: always
ports:
- "3306:3306" # port mapping
environment:
MYSQL_ROOT_PASSWORD: 1234 # root 비밀번호
volumes:
- ./data:/var/lib/mariadb # docker-compose.yml 있는곳에 데이터 저장

```

### docker-compose 실행

```
# docker-compose.yml 있는곳에서 아래 command 실행
$ docker-compose up -d # detach(background)로 실행
```

### mariadb container 접속

```
$ docker exec -it [name] bash
$ mysql -u root -p
Enter password: 
MariaDB > create database wbdev;
```