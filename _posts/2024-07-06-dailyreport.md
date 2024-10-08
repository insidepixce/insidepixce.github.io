---
pin: true
layout: post
title: "맥에서 리눅스 환경 구축하기- 도커란 무엇인가?s"
categories:
  - javascript
tags:
  - javascript
author: insidepixce
show_excerpts: true
date: 2024-07-06 10:00:00 +0900
disqus: true
paginate: true
disqus-count-scr: true
comments: true
toc: true

---
도커란 무엇인가? 개념부터 정리해보자. 


# intro
보안을 다뤄야 하는 상황인데, 나는 오직 맥북만 가지고 있다. 심지어 내 맥북은 intel도 아닌 m2.
기본적인 리눅스 환경을 구축해야 하는데... 나한테는 방법이 없는 상황인걸까? 
윈도우 pc를 하나 더 사고 설정하기 귀찮았던 나는 docker로 리눅스를 쓰겠다는 다소 무모한 도전을 시작하기로 했다. 

# 어떻게 리눅스 환경을 구축할것인가 
먼저 도커를 설치하기로 한만큼 , 도커에 대해서 알아보자. 

## 도커란? 
도커는 소프트웨어 개발 및 배포에 혁신을 가져온 플랫폼이라고 한다. 애플리케이션을 컨테이너라는 가벼운 가상 환경에서 실행할 수 있도록 도와주고, 이러한 컨테이너는 격리된 환경에서 애플리케이션과 그 종속성을 함께 패키징하여 어디서든 실행할수 있게 도와준다. 

## 비유해보자면
도커는 일종의 컨테이너 선박으로 비유할 수 있다. 

### 컨테이너
컨테이너는 물건을 담는 상자이다. 컨테이너는 일정한 크기와 형태를 가지고 있으며, 어떤 물건이든 그 안에 넣을 수 있다. 
즉 그냥 컨테이너는 진짜 컨테이너라고 생각하면 된다는 이야기이다. 컨테이너 상자에 물건을 넣으면, 그 상자는 배든 기차든 트럭이든 동일하게 운반할 수 있다. 이와 같이 그 애플리케이션은 어떤 컴퓨터에서 동일하게 실행될 수 있다는거다. 

### 이미지 
컨테이너에 어플리케이션 채워 넣기 전에 어떻게 채울지를 정하는 설계도나 템플릿이다. 설계도에 따라 컨테이너 상자에 물건을 넣는 것 처럼 말이다, 

### 도커 엔진 
컨테이너 선박을 운영하고 관리하는 시스템이다. 도커 엔진은 컨테이너를 생성하고 실행하는 소프트웨어라고 생각하면 된다. 나도 이게 잘 이해가 안되었다, 

### 도커 허브 
컨테이너를 보관하고 필요할떄 꺼내 쓸수있는 큰 창고이다. 이미지들을 저장하고 공유하는 클라웅드 서비스이다. 
물건을 보관해두는 창고처럼, 도커 허브는 도커 이미지를 저장해두고 필요할때 다운로드받아 사용할 수 있다 . 

### 도커 컴포즈 
여러개의 컨테이너 상자를 묶어 한번에 관리하는 시스템이라고 보면 된다 
여러 컨테이너 상자를 하나의 운송 단위로 묶어 관리하는 느낌? 

### 그니까...
- 컨테이너 : 물건을 담은 상자 (애플리케이션과 모든 필요요소를 포함한 패키지)
- 이미지 : 상자를 채우기 위한 설계도(컨테이너 생성 템플릿)
- 도커 엔진 : 선박 운영 시스템 (컨테이너를 생성하고 관리하는 소프트웨어)
- 도커 허브 : 큰 창고 (도커 이미지를 저장하고 공유)
- 도커 컴포즈 : 상자 묶음 관리 시스템 (여러 컨테이너를 정의하고 실행)