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
도커로 리눅스 서버를 띄워보자. 이걸로 실습 다 해내고 말테야. 도대체 왜 나는 앱등이일까? 맥북 짱 편한데 왜 보안에서는 맥북을 사용하기가 더럽게 힘든걸까>? 

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
아까 공부한 대로 
```
docker run -it ubuntu /bin/bash
```
이 명령어를 통해 서버를 시작해주면 

<img width="936" alt="스크린샷 2024-07-07 오전 5 28 09" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/2309cf7d-9f05-4afa-bc3e-76a14a66203b">
이러한 프로세스와 함께 뭐가 많이 뜰것이다. 

mkdir (디렉토리 이름)
```
cd (디렉토리 이름)
```
<img width="468" alt="스크린샷 2024-07-07 오전 5 31 48" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/83692fec-9bfc-4c9a-b2d4-9c660d81cc67">

프론트앤드 개발할때 터미널에서 사용하던 명령어와 같아서 보기가 좀 편했다.
cd는 해당 디렉토리로 이동하라는 말이였고, mkdir은 해당 디렉토리를 생성하라는 이야기이다. 
이 상태에서 도커파일을 꺼내보자. 

### 2. 도커파일 생성
```
nano Dockerfile
```
<img width="570" alt="스크린샷 2024-07-07 오전 5 37 15" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/0ff9fd0b-441b-42a6-bab8-f430a1a69e62">
nano가 안 깔려있어서 이렇게 뜬다

```
vi Dockerfile
```
둘 중 하나를 선택해서 하면 되는데, 나는 앞서 vim만 설치해두었다. 
그래서 vi를 사용하여 진행해보려고 하였으나 nano도 찍먹해보고싶어서 설치했다 
타 개발자분들을 봤을때 vim을 많이 사용하시는것 같아서 앞으로는 vim 위주로 사용햐보려고 한다 


<br>

***nano 설치하기***
```
apt update
apt install -y nano
```

### 3. dockerfile에 이걸 복사붙여넣기 추가한다. 
언젠가는 나도 이 내용을 하나하나 만져가면서 이해하는 날이 올거라 믿는다. 일단은 주어진 내용만으로도 열심히 이해해보도록 하자 
```
# 최신 Ubuntu 이미지를 기반으로 사용
FROM ubuntu:latest

# 패키지 목록을 업데이트하고 OpenSSH 서버를 설치
RUN apt update && apt install -y openssh-server

# 루트 비밀번호 설정
RUN echo 'root:password' | chpasswd

# SSH를 통해 루트 로그인 허용
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# 권한 분리 디렉토리 생성
RUN mkdir -p /run/sshd

# 포트 22 노출
EXPOSE 22

# SSH 서비스를 시작
CMD ["/usr/sbin/sshd", "-D"]
```

1. from ubuntu 
- 최신 우분투 이미지를 기반으로 사용한다는 이야기다. `from` 지시어는 docker 이미지의 기본 이미지를 지정한다
2. run apt update && apt install -y openssh-server
-  `openssh-server`패키지를 설치한다. `apt update`는 패키지 목록을 업데이트하고 `apt-install -y openssh-server` 는 상호작용 없이 openSSH 서버를 설치한다...
이때 상호작용 없이라는 말은 예를 들어 
```
Do you want to continue? [Y/n]
```
이걸 모두 건너뛴다는 것이다. -y라고 했기 떄문에 yes로 설정해놓는다는 이야기.
이걸 안해놓으면 대참사가 일어난다 
```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  ncurses-term openssh-sftp-server
Suggested packages:
  ssh-askpass molly-guard rssh monkeysphere
The following NEW packages will be installed:
  ncurses-term openssh-server openssh-sftp-server
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 1,238 kB of archives.
After this operation, 5,872 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```
이걸 하나하나 다 y를 눌러야 한다는 점. 


3. RUN echo 'root:비밀번호' | chpasswd
- 루트 사용자 계정의 비밀번호를 설정한다. 나는 귀찮아서 그냥 password를 비밀번호로 해두었다!
4. RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config\
- ssh 설정 파일 (/etc/ssh/sshd_config)에서 PermitRootLogin 설정을 변경하여 루트 로그인을 허용한다.
<br>
- sed -i 명령어는 파일을 직접 수정하는데, #PermitRootLogin prohibit-password를 PermitRootLogin yes로 바꾼다

5. RUN mkdir -p /run/sshd
- ssh 권한 분리 디렉토리를 생성한다.

6. expose 22
- docker 컨테이너가 사용하는 포트 22를 올린다. expose 지시어는 docker 컨테이너가 외부와 통신하기 위해 사용하는 포트를 지정한다

7. cmd ["/usr/sbin/sshd", "-D"]
- ssh 서비스를 데몬 모드로 시작한다. `cmd`지시어는 컨테이너가 시작될 때 실행할 명령어를 지정한다. 여기서는 `sshd`를 데몬 모드(`-d`)로 실행하여 컨테이너가 종료되지 않도록 한다. 

이 도커파일은 우분투 기반의 도커 이미지를 빌드하고 `openssh` 서버가 설치되고 설정된 컨테이너를 실행할 수 있다. ssh 를 통해 루트 계정으로 접속할 수 있도록 설정되어 있으며 , 포트 22를 외부에 올려놔서 ssh 접속을 허용한다 

<br>
그래도 좀 안면이 있는 vim으로 하기로 결정
<img width="1516" alt="스크린샷 2024-07-07 오전 6 03 20" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/42e17121-38da-4709-91cc-c0124820d04f">

```
vi Dockerfile
```
해당 명령어를 치면 나오는 곳이다. 
이곳에 아까 그 코드들을 복사 붙여넣기 해주면 되는데, 
번외편으로 vim 사용법을 짚고 넘어가도록 해보겠다 

# vim의 기본모드 
1. 명령 모드 : vim이 시작될때의 기본 모드이다. 텍스트를 입력하지 않고 명령어를 입력할 수 있다.
2. 입력 모드 : 텍스트를 입력할 수 있는 모드이다. 명령 모드에서 i를 눌러 입력 모드로 전환한다. 
3. 명령-라인 모드 : 파일 저장, 종료 등의 명령어를 입력할 수 있는 모드이다. 명령 모드에서 : 를 눌러 진입한다

> 여기서 명령 모드와 입력 모드의 차이점이 잘 구별이 안 갔는데, 알고보면 굉장히 쉽다. 
## 여러가지 단축키

### 입력 모드로 전환

- i : 현재 커서 위치 앞에 텍스트를 입력한다
- a : 현재 커서 위치 뒤에 텍스트를 입력한다.
- o : 현재 커서 아래에 새로운 줄을 추가하고 텍스트를 입력한다.

### 입력 모드에서 명령 모드로 전환
- 입력 모드에서 Esc 키

### 파일 저장 및 종료
- 명령 모드에서 :를 눌러 명령-라인 모드로 전환

- :w : 파일을 저장
- :q : vim을 종료
- :wq 또는 :x : 파일을 저장하고 vim을 종료
- :q! : 변경 사항을 저장하지 않고 vim을 강제로 종료

### 텍스트 편집
명령 모드에서도 텍스트를 편집할 수 있다. 

- dd : 현재 줄을 삭제
- yy : 현재 줄을 복사
- p : 복사하거나 잘라낸 내용을 현재 커서 위치에 붙여넣기
- u : 마지막 명령을 취소
- Ctrl + r : 마지막 명령을 다시 실행

>여기서 명령 모드와 입력 모드의 차이점이 드러나는 것이다. 
>내용 수정은 입력 모드에서만 가능하다


vim에 접속한 후 i를 누르면 
<img width="533" alt="스크린샷 2024-07-07 오전 6 11 28" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/f9ca0d5b-b3b2-44c7-8b71-610e1e4e19cb">
이렇게 insert 마크가 뜬다. 이때부터 편집하면 된다.

<img width="597" alt="스크린샷 2024-07-07 오전 6 15 21" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/458f2aa2-6653-4e57-92d6-8e9c801dcea1">
이해를 돕기 위한 주석을 전부 제거하고 입력해주었다. 
esc키를 누르고 :wq를 입력후 나오면 된다

---
### 3. docker 이미지 빌드
dockerfile을 사용하여 docker 이미지를 빌드한다.
```
exit
```
먼저 이렇게 입력해서 나가줘야 한다
<img width="293" alt="스크린샷 2024-07-07 오전 6 26 48" src="https://github.com/insidepixce/insidepixce.github.io/assets/126161716/68a51e59-6ef6-4414-bd85-27e64a55db6c">
현재 우리는 로컬 머신에서 도커 컨테이너를 실행하고 해당 컨테이너 안에 있는 상황이였다. 도커 명령어가 작동하지 않는 이유는 도커 데몬이 컨테이너 내에서는 실행되지 않기 떄문이다.
컨테이너 내부에 들어가서 docker 명령어를 실행하려고 하면 컨테이너 안에서는 도커 데몬이 실행되고 있지않기 떄문에 
command not find 오류가 발생한다. 

컨테이너 내부에서 도커 명령어를 사용하려면 도커 호스트 *로컬 머신 으로 돌아가야 한다. 컨테이너 내부에서 'exit'명령어를 입력하여 호스트로 돌아갈 수 있다.

아 맞다 , 중요한 점이 있다. 
# 도커는 한번 나가면 저장이 되지 않는다 
이걸 왜 이제야 말해주냐고요ㅕ? 저도 당했거든요 . 

그럼 도커에서 작업 후 어떻게 저장하냐면...

## 1.이미지로 커밋하기:
컨테이너 내에서 작업한 내용을 새로운 도커 이미지로 커밋할 수 있다.

```
docker commit <컨테이너_ID> <새로운_이미지_이름>
```
예를 들어, my-ubuntu-ssh-modified라는 이름의 새로운 이미지를 만들려면 다음과 같이 실행합니다:
```
docker commit <컨테이너_ID> my-ubuntu-ssh-modified
```

## 2. 데이터 볼륨 사용하기:
중요한 데이터를 컨테이너 외부에 저장하고 싶다면 데이터 볼륨을 사용할 수 있다. 
데이터 볼륨은 호스트 시스템과 컨테이너 간에 데이터를 공유할 수 있도록 해준다.
```
docker run -v /path/on/host:/path/in/container -it ubuntu:latest /bin/bash
```
예를 들어, 호스트의 /data 디렉토리를 컨테이너의 /data 디렉토리로 마운트하려면
```
docker run -v /data:/data -it ubuntu:latest /bin/bash\
```

## 3. 컨테이너 중지 후 재시작하기:
exit 명령어 대신 docker stop 명령어로 컨테이너를 중지한 후 docker start 명령어로 재시작할 수 있다. 

```
docker stop <컨테이너_ID>
docker start <컨테이너_ID>
```

## 4. Dockerfile 사용하기:
반복적으로 수행할 작업이나 설치할 패키지가 있다면, Dockerfile에 이러한 작업을 기록해두고 필요할 때마다 이미지를 빌드하여 사용할 수 있다. 
이를 통해 항상 동일한 환경을 재현할 수 있다.

```
docker build -t insidepixce-test .
```

나는 4번째 방법을 채택해볼것이다. 

### 1. dockerfile 생성
먼저 dockerfile을 생성할 디렉토리부터 만들어보자 
```
mkdir my-docker-image
cd my-docker-image
```

### 2. dockerfile 작성
텍스트 에디터를 이용해 Dockerfile이라는 이름의 파일을 생성하고 밑에 내용을 추가한다
```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y vim nano
CMD ["/bin/bash"]
```
베이스 이미지로 `ubuntu`를 사용하고, 패키지 목록을 업데이트하고 `vim`과 `nano`를 설치한다. 
마지막으로 컨테이너가 실행될 떄 기본적으로 bash를 실행하도록 설정한다. 

### 3. 도커 이미지 빌드
작성된 도커파일을 기반으로 이미지를 빌드한다. 'my-custom-image'는 이미지 이름이다. 
```
docker build -t my-custom-image .
```

### 4. 컨테이너 실행
빌드한 이미지를 사용하여 컨테이너를 실행한다
```
docker-run -it my-custom-image
```
>여기서 복습 한번 해보자면 it는 어떤 어떤 옵션을 결합한 명령어였을까? 컨테이너의 표준 입력을 활성화하여 터미널 입력을 받을 수 있게 하고, 터미널을 할당하여 컨테이너 내부에서 터미널 세션을 사용할 수 있게한다고 나와있었다. 이 옵션은 터미널 사용자 인터페이스를 제공하는데 필요한 옵션이다. 까먹지 말자 ! 하나씩 기억해 나가는거야. 
