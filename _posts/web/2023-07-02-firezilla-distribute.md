---
title:  "FireZilla를 이용한 배포 (SpringBoot, Vue.js)"
excerpt: "첫 배포 기념 정리"

categories:
  - Web
tags:
  - [Web, Distribution, FireZilla]

toc: true
toc_sticky: true
 
date: 2023-07-02
last_modified_at: 2023-07-02
---

<br>


# **0. 사전 준비**

<br>

## **PuTTY**

<br>

- linux, unix 계열 서버에 원격으로 접속할 수 있는 프로그램
    - Windows에서 Ubuntu 이용하기 위해 설치
- PuTTY 다운로드 : [https://www.putty.org/](https://www.putty.org/)
    - putty.exe, puttygen.exe 다운로드
    - mac은 terminal에서 가능
- 사용법
    - SSH > Auth > Credential > Private key file for authentication에 ppk key를 열기
    - Session > Host Name for IP address : EC2의 퍼블릭 IPv4 주소
    - open > Accept 하면 터미널 창 하나 나옴
    - login as : ubuntu

<br>

> 💡 **[Windows Chocolatey](https://chocolatey.org/install)**
>
> - Windows에서 사용할 수 있는 커멘드라인 패키지 매니저
>    - Choco를 이용하여 Windows에서 OpenSSH 사용할 수 있음
> - 설치 : Windows PowerShell을 관리자로 실행하고 아래 명령어 입력
>    
>    ```powershell
>    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
>    ```

<br>
<br>

## **pem vs. ppk**

<br>

- pem : Open SSH 용 (mac, linux 등 os 사용하는 경우)
- ppk : PuTTY에서 사용
- PuTTY Gen : pem을 ppk로 바꿔줌
    - 사용법 : Load > pem key 선택하고 열기 > 확인 > Save private key > 예 클릭하여 저장

<br>
<br>

# **1. 배포**

<br>

## **ubuntu 가상 컴퓨터 환경에 필요한 패키지 설치**

<br>

PuTTY(ubuntu)에서

- 다운로드 받을 수 있는 패키지를 업로드
    
    ```powershell
    sudo apt-get update
    ```
    
- 필요한 개발환경과 DB 설치
    - Zulu 8 : [https://docs.azul.com/core/zulu-openjdk/install/debian](https://docs.azul.com/core/zulu-openjdk/install/debian)
        
        ```powershell
        # Public key import
        $ sudo apt install gnupg ca-certificates curl
        $ curl -s https://repos.azul.com/azul-repo.key | sudo gpg --dearmor -o /usr/share/keyrings/azul.gpg
        $ echo "deb [signed-by=/usr/share/keyrings/azul.gpg] https://repos.azul.com/zulu/deb stable main" | sudo tee /etc/apt/sources.list.d/zulu.list
        
        # 패키지를 추가했으니까 한 번 더 업데이트
        $ sudo apt update
        
        # Zulu 8 설치 : 다른 버전 설치하는 경우는 숫자를 변경
        $ sudo apt install zulu8-jdk
        
        # 설치 후 버전 확인
        $ java -version
        ```
        
    - MariaDB (aws DB 사용하는 경우에는 진행 X)
        
        ```powershell
        sudo apt-get install mariadb-client mariadb-server -y
        sudo mysql_secure_installation
        ```
        
        
        > 💡 -y : 패키지 설치할 때 y 입력(yes) 하는 부분 생략
        
        
        ```powershell
        Enter current password for root (enter for none) # enter (아직 설정하지 않았으므로)
        Switch to unix socket authentication [Y/n] # Y
        Change the root password? [Y/n] # Y 후 사용할 password 입력
        Remove anonymous users? [Y/n] # Y (보안상 항상 제거를 권장)
        Disallow root login remotely? [Y/n] # Y
        Remove test database and acces to it? [Y/n] # Y
        Reload privilege tables now? [Y/n] # Y
        ```
        
        - 설치 후 확인해보기
            
            ```powershell
            mysql -u root -p # root로 로그인하고 비밀번호 입력한다
            exit # 나갈 때
            ```
            
<br>
<br>

## **FileZilla에 가상컴퓨터 세팅**

<br>

- FileZilla : 원격으로 파일을 업로드하고 다운로드할 수 있는 FTP(File Transfer Protocol) 클라이언트
- 다운로드 : [https://filezilla-project.org/](https://filezilla-project.org/)
- FileZilla에 가상컴퓨터 환경 불러오기
    - 사이트 관리자 > 새 사이트 클릭 후 사이트 이름 설정
        - 프로토콜 : SFTP (보안 고려)
        - 호스트 : EC2 인스턴스의 퍼블릭 IPv4 주소
        - 로그온 유형 : 키파일
        - 사용자 : ubuntu (서버에 접속하는 계정 이름)
        - 키 파일 : pem 또는 ppk key 파일
        
        이후 연결
        
<br>
<br>

## **SpringBoot 배포**

<br>

- jar 파일, sql 파일을 ubuntu 하위로 옮긺
    
    
> 💡 **SpringBoot -> jar** [(참고)](https://xzio.tistory.com/1401)
>
>- 프로젝트 선택 > Run as > Maven build
>    - Goals : package
>    - Profiles: pom.xml
>    
>    이후 Run
>    
>- Build Success 뜨면 성공
>- 빌드 파일 경로 : 콘솔 로그 중 Building jar : 에 출력됨
    
- ubuntu에서 ls로 파일 확인
- 가상컴퓨터의 mysql에 sql 쿼리 넣어주기 (aws DB 사용하는 경우에는 진행 X)
    
    ```powershell
    mysql -u root -p < init <sql 파일명.sql>
    show database; # 넣은 데이터베이스 확인
    ```
<br>
<br>    

## **nginx 설치 (웹 서버)**

<br>

- nginx : Apache와 같은 웹 서버 프로그램
- nginx 설치
    
    ```powershell
    sudo apt-get install nginx-core -y
    ```
    
- nginx 명령어
    - `sudo systemctl status nginx` : 서버 상태 보기 (active라고 나와야 함)

<br>
<br>

## **Vue 배포**

<br>

- 터미널에서 vue 파일이 있는 경로로 이동
- vue build
    
    ```powershell
    npm // npm 설치 확인
    npm i // node modules 설치
    npm run build // static 파일로 바꿔줌
    // Build Complete
    ```
    
    - 생성된 dist 폴더를 압축
- ubuntu에 dist.zip을 옮긺
- ubuntu에서 unzip
    
    ```powershell
    sudo apt get install unzip // unzip이 설치되어 있지 않은 경우 설치
    mkdir dist // dist 폴더에 unzip하기 위해 dist 폴더 만들어줌
    unzip dist.zip -d dist/. // -d 옵션 이용하여 dist 폴더에 unzip
    ```
    
- 서버 주소에서 프론트 볼 수 있도록 해당 폴더에 복사
    
    ```powershell
    sudo cp -r * /var/www/html/. // /var/www/html/.에 파일을 복사 (-r 하위에 있는 것까지 모두)
    ```

<br> 


> 🚨 **주의 사항**
>
>- css를 하나의 .css 파일로 만들기
>    - 각각의 vue에 만들면, 빌드 과정에서 하나의 css 파일로 만들어져서 적용 우선순위가 달라질 수 있음 [(참고)](https://github.com/lumiamitie/TIL/blob/master/js/vue_css_differ_prod_dev.md)

<br>
<br>

## **SpringBoot와 연동**

<br>

- 서버 돌리기
    
    ```powershell
    cd ~ // home으로 이동
    ls // 전체 파일 확인
    java -jar <jar 파일명.jar> // 서버 실행. 맨 앞 일부만 입력하고 tab 누르면 자동완성 됨
    // 서버 끄기 : ctrl + c
    ```
    
- default 설정 파일에 location 추가
    
    ```powershell
    cd /etc/nginx/sites-available/
    ls // default라는 파일이 하나 있음
    // default 파일 수정. 이 위치는 항상 관리자 권한 필요
    sudo vim default
    
    // location을 추가. 아래처럼 쓰면 /api로 들어오는건 localhost 9999로 보낸다는 의미 
    location /api {
    	proxy_pass http://localhost:9999;
    }
    :wq
    
    // 설정 파일을 수정해서 nginx 껏다가 켜기
    sudo systemctl stop nginx
    sudo systemctl start nginx
    sudo systemctl status nginx // active 잘 되었는지 확인
    
    // 서버 다시 실행
    cd ~
    java -jar <jar 파일명.jar>
    // 이제 IPv4 주소로 들어가면 화면을 확인할 수 있음
    ```

<br>
<br> 

## **nohup으로 서버를 백그라운드에서 실행**

<br>

- nohup : 터미널과의 세션 연결이 끊기더라도 프로세스를 계속 동작 시킴 (No Hang Up)

```powershell
nohup java -jar <jar 파일명.jar> & // & : 프로그램을 백그라운드로 실행

// 서버 끄려면
ps -ux | grep "java" // -ux 결과 중 "java"에 해당하는 것만 보여줘
kill -9 <pid> // PID로 kill
```

<br>
<br>