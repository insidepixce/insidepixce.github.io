---
pin: true
layout: post
title: "맥에서 리눅스 환경 구축하기- 도커 기본 설정값 1편"
categories:
  - knockon
tags:
  - knockon
author: insidepixce
show_excerpts: true
date: 2024-07-06 11:00:00 +0900
disqus: true
paginate: true
disqus-count-scr: true
comments: true
toc: true

---

# 개요
저번 포스팅에서 도커란 무엇인지 알아보았으니 이번 포스팅에서는 도커를 사용해 리눅스 서버를 실행해보자.
사실 내가 아는 많은 보안 전문가들이 m1을 이용하여 작업을 하지 않는다는걸로 보아 내 시도가 부질없을 확률이 크지만 일단은 임시방편으로는 쓸 수 있을 것 같기도 하고 이 과정을 통해 내가 뭐라도 배우겠지 싶어서 무모한 도전에 뛰어들었다. 


## steps

1. 도커 설치하기 
- 도커 다운로드 및 설치 
(https://www.docker.com/)에서 도커를 설치할 수 있다. 
굳이 공식 홈페이지에 들어가서 설치하지 않더라도 mac os 를 조금이라도 다뤄봤다면 설치되어 있을 ***homebrew*** 를 통해 설치할 수 있다.

```
brew install --cask docker
```
- brew-> 홈브류를 실행하는 명령어. 
- install -> 소프트웨어를 설치하라는 명령어. 
- --cask -> 홈브류의 cask를 사용하라는 옵션이다. homebrew의 cask는 gui어플리케이션을 설치하고 관리하기 위해 사용되는 homebrew의 확장이다. 
- docker -> 설치하려는 소프트웨어 패키지의 이름이다 

2. 도커 실행 확인
- 터미널을 열고 명령어를 입력하여 도커가 정상적으로 설치되어있는지 확인해준다. 
```
docker --version
```
<img width="763" alt="스크린샷 2024-07-06 오후 9 32 56" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/cf28dfa6-a063-46df-9d27-c45d2d4cf423">
나는 커스텀하는걸 굉장히 좋아하기 때문에 터미널도 이렇게 커스텀을 해주었다. 원래는 일반 터미널로 들어가서 해야함 !!

<img width="388" alt="스크린샷 2024-07-06 오후 9 34 20" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/c8b5acaa-f64e-4ddc-aa09-2b2f0d6a007a">

이렇게 도커의 버전이 확인이 된다 

3. 도커 이미지 다운로드 
```
docker pull ubuntu
```

원하는 리눅스 이미지를 도커 허브에서 다운로드한다. 예를 들어 ubuntu 이미지를 다운로드해보자. 솔직히 어떤걸 해야할지 몰라가지고 걍 아무거나 했다. 
<img width="497" alt="스크린샷 2024-07-06 오후 9 35 58" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/770067bc-0f99-4313-b957-df0f8d9b4f1a">
만만한 우분투 소환. 이 포스팅을 작성하기 전 필자는 이전에 설치를 해두어서 코드들이 막 폭포마냥 쏟아져내려가진 않는데, 원래는 뭔 프로세스가 미친듯이 작동한다. 

4. 컨테이너 실행 
- 다운로드한 이미지를 이용하여 컨테이너를 실행한다. 
```
docker run -it ubuntu /bin/bash
```
- docker -> 도커 명령어를 실행하는 도구 
- run -> 새로운 컨테이너를 생성하고 실행하는 명령어이다 
- -it -> 두개의 옵션을 결합한 것이다 
    -> -i (interective): 컨테이너의 표준 입력을 활성화하여 터미널 입력을 받을수 있게 한다 
    -> -t (tty): 터미널을 할당하여 컨테이너 내부에서 터미널 세션을 사용할 수 있게 한다. 이 옵션은 터미널 사용자 인터페이스를 제공하는데 필요하다. 
- ubuntu ->  사용할 도커 이미지의 이름이다. 여기서는 우분투 이미지를 사용했고, 이 이미지는 도커 허브에서 자동으로 다운된다. 아까 다운로드를 했기 때문에 우리는 추가적으로 다운로드 되지 않는다 
- /bin/bash -> 컨테이너가 실행될때 시작할 명령어를 뜻한다. 여기서는 ubuntu 컨테이너에서 bash 셀을 실행하도록 지정한다. 이를 통해 컨테이너 내부에서 명령어를 입력하고 실행할 수 있다.

 > ℹ️ 즉 docker run -it ubuntu /bin/bash 명령어는 Ubuntu 도커 이미지를 사용하여 새로운 컨테이너를 생성하고, 그 컨테이너 안에서 인터랙티브 Bash 셸을 실행한다. 

 >🔥 이 명령어를 실행하면 터미널이 컨테이너 내부의 bash 셀로 전환되어 사용자가 직접 명령어를 입력하고 실행할 수 있는 환경을 제공한다 

5. 컨테이너 내에서의 작업
컨테이너가 실행되면, 터미널이 ubuntu 셀로 바뀐다. 이제 리눅스 명령어를 사용하여 필요한 작업을 할 수 있따.

나는 제일 먼저 vim을 설치해주었다

```
apt-get update
apt-get install vim
```
프론트앤드 개발자였던 필자의 입장에서 보기에는 `apt-get` 과 자바스크립트의 `yarn` 그리고 `npm` 
이 꽤나 유사하게 보였다. 
<img width="1521" alt="스크린샷 2024-07-07 오전 5 23 37" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/c3fb83aa-d0be-4c6a-a64f-86b57ab78ba6">
이렇게 프로세스가 뜬다 

그렇게 vim을 설치해주고, 

6. 컨테이너 종료
```
exit
```
작업이 끝나면 컨테이너를 종료햐주면 된다

7. 실행 중인 컨테이너 확인
```
docker ps
```
여기서 ps는 `process status` 를 의미하는데, 이는 유닉스 및 리눅스 시스템에서 프로세스 상태를 보여주는 명령어인 `ps`에서 유래하였다. 현재 실행중인 도커 컨테이너의 정보를 출력하며, 기본적으로 실행 중인 컨테이너만 표시한다.


### 여러가지 옵션들
- `-a` 또는 `--all` : 모든 컨테이너(실행 중이거나 중지된 컨테이너 모두) 
- `-q` 또는 `--quiet`: 컨테이너 id만 출력한다
- `-f` 또는 `--filter`: 특정 조건에 맞는 컨테이너만 필터링하여 표시한다

### 예시

실행 중인 모든 컨테이너 표시:
```
docker ps
```

모든 컨테이너(실행 중이거나 중지된 것 포함) 표시:
```
docker ps -a
```

컨테이너 ID만 출력:
```
docker ps -q
```

특정 이미지로 생성된 컨테이너만 표시: 
```
docker ps -f ancestor=ubuntu
```

---
자 이제 도커 사용법도 대충 알아봤으니 이제 이걸로 리눅스 서버를 띄워보자 !
---


# 리눅스 서버를 띄워보자 

## dockerfile 작성

### 1. 터미널을 열고 작업할 디렉토리 만들기 
