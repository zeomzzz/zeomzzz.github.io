---
title:  "Docker로 설치한 Jenkins 업데이트"
excerpt: ""

categories:
  - Infra
tags:
  - [Infra, Jenkins, Docker]

toc: true
toc_sticky: true
 
date: 2023-10-12
last_modified_at: 2023-10-13
---

<br>

### **방법 1. jenkins.war를 받아서 설치한 경우**


>💡 **사전확인 사항 (wget: command not found)**
>
>1. wget 설치 확인 : `which wget`
>2. 없다면 wget 설치 : `apt-get install wget`
>- mac은 apt-get이 아닌 brew(Homebrew) 패키지 관리자를 사용   
>    → `brew install wget`
    
<br> 

1. 컨테이너에 root 권한으로 접속 : `docker exec -u root -it <컨테이너 명> bin/bash`
    
    ex. `docker exec -u root -it jenkins-server bin/bash`
    
    - 컨테이너 이름 확인 : `docker ps`
2. 업데이트할 jenkins를 wget을 이용하여 다운로드 : `wget http://updates.jenkins-ci.org/download/war/<버전>/jenkins.war`

<br> 
<br> 

### **방법 2. (이걸로 해결)**

- 주의 : 필요하다면 백업 !!
1. 새로운 Jenkins 이미지 다운로드 : `docker pull jenkins/jenkins:lts`
2. 기존 컨테이너를 중지하고 삭제 : `docker stop jenkins` > `docker rm jenkins`
3. 새로운 이미지로 컨테이너를 실행 : `docker run -d -p 8080:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts`
    - `--name jenkins` : jenkins라는 이름으로 실행. 원하는 이름이 있는 경우 변경하기
  
<br> 