---
title:  "Docker로 설치한 Redis의 데이터가 초기화되는 오류"
excerpt: ""

categories:
  - Infra
tags:
  - [Infra, Docker, Redis, TroubleShooting]

toc: true
toc_sticky: true
 
date: 2023-11-10
last_modified_at: 2023-11-10
---

<br>

# **1. 문제 상황**

<div class="descipt-font">
서버가 빌드될 때마다 Redis Container도 Stop - Start를 하고 있었는데, <br> Container 재실행 과정에서 데이터가 손실되고 있지는 않은지 우려되어 확인해보았다. <br> 역시나 Redis의 data가 초기화되고 있었다 ..!
</div>

<br>

# **2. 원인 파악**

- 서버가 빌드 될 때마다 Redis container도 docker down 후 다시 docker up 되고 있었다.
    
    ![]({{ site.url }}{{ site.baseurl }}/assets/images/infra/redis-docker-compose/01.png ){: .align-center}
    
    - 서버와 Redis가 하나의 docker-compose.xml에서 실행되고 있었다.
    - 그렇지만 서버를 빌드할 때마다 Redis를 다시 실행할 필요가 없었다.

<br>

# **3. 해결 방법**

- 서버를 빌드할 때 Redis도 다시 실행하지 않도록 Redis docker-compose를 서버 docker-compose와 분리해서 ec2 내의 다른 위치에 저장했다.
    - 동일 디렉토리 내에 저장하여 `.env`를 공유하도록 했다.
        - docker-compose.yml
            
            ```yaml
            services:
            		gunpang:
            				build: .
                    container_name: [서버 Container 이름]
                    environment:
            						DATABASE_PASSWORD: ${DATABASE_PASSWORD}
                        DATABASE_URL: ${DATABASE_URL}
                        DATABASE_USERNAME: ${DATABASE_USERNAME}
                        JWT_SECRET: ${JWT_SECRET}
                        REDIS_HOST: ${REDIS_HOST}
                        REDIS_PORT: ${REDIS_PORT}
                        REDIS_PASSWORD: ${REDIS_PASSWORD}
                        TZ: Asia/Seoul
            				ports:
            		        - "${BACK_PORT}:${BACK_PORT}"
                    restart: always
            ```
            
        - docker-compose-redis.yml
            
            ```yaml
            services:
            		redis:
            			  image: redis:latest
                    container_name: [Redis Container 이름]
                    restart: always
                    ports:
            		        - [Redis Port]:6379
                    environment:
            		        - REDIS_PASSWORD=${REDIS_PASSWORD}
                        - TZ=Asia/Seoul
            ```
            
- 서버를 빌드할 때 Redis는 다시 실행되지 않고, 데이터도 유지되는 것을 확인했다.
    - 빌드 전 Redis
        
        ![]({{ site.url }}{{ site.baseurl }}/assets/images/infra/redis-docker-compose/02.png ){: .align-center}
        
    - 빌드
        
        ![]({{ site.url }}{{ site.baseurl }}/assets/images/infra/redis-docker-compose/03.png ){: .align-center}
        
    - 빌드 후 Redis : 데이터 유지됨
        
        ![]({{ site.url }}{{ site.baseurl }}/assets/images/infra/redis-docker-compose/04.png ){: .align-center}
        
<br>

# **4. 느낀점**

- Redis를 Docker로 띄우는 거보다는 직접 띄우는 것이 좋을 것 같다
    - Redis를 편리하게 사용하기 위해서 Docker를 이용했다.
    - 그런데 Docker Container가 중지되면 데이터 손실이 발생할 수 있어서 데이터를 저장하는 용도로는 적합하지 않은 것 같다.
- Docker를 이용하는 경우, Redis 데이터를 서버 내에 백업하는 방법을 찾아봐야겠다.
    - Container 중지로 인한 데이터 손실을 최소화하기 위해서

<br>
