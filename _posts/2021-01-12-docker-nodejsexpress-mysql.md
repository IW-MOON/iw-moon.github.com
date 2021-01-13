---
title: Docker + Node.js(Express) + Mysql  컨테이너 환경 구성
categories:
- Project
---

<br>
![수정됨_server_archi](https://user-images.githubusercontent.com/72685070/104442844-fbc1a180-55d8-11eb-93f4-1683b51ec9c9.png)                     
<br><br><br>







### **컨테이너 환경구성**
---
![docker](https://user-images.githubusercontent.com/72685070/104443296-9c17c600-55d9-11eb-8db0-9834c2751edc.PNG)


> (1) docker-compose.yml

```javascript
version: '3'
services:
  proxy:
    image: nginx:latest   # 최신 버전의 Nginx 사용
    container_name: proxy # container 이름은 proxy
    ports: 
     - "80:80"           # 80번 포트를 host와 container 맵핑
     - "443:443"           # 443번 포트를 host와 container 맵핑
    volumes:
     - ./proxy/nginx.conf:/etc/nginx/nginx.conf # nginx 설정 파일 volume 맵핑
     - /etc/nginx/ssl:/etc/nginx/ssl
    restart: "unless-stopped" # 내부에서 에러로 인해 container가 죽을 경우 restart
  express1:
    build:
      context: ./server  # 빌드할 Dockerfile이 위치한 경로
    container_name: express
    expose:
      - "3000"           # 다른 컨테이너에게 3000번 포트 open
    volumes:
      - ./source:/source # host <-> container의 source 디렉토리를 공유
      - /source/node_modules 
    restart: "unless-stopped"
```

> (2) Dockerfile

```javascript
FROM node:12

WORKDIR /source
COPY . .

RUN npm install
#RUN npm install express mysql
RUN npm install -g nodemon
#RUN npm install aws-sdk
CMD [ "nodemon", "-L", "app.js"]
```

> (3) package.json

```javascript
{
  "name": "node-exam",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "debug": "~2.6.9",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "mysql": "^2.17.1",
    "aws-sdk": ">= 2.0.9",
    "jsonwebtoken": "^7.1.9",
    "winston": "^3.3.3",
    "winston-daily-rotate-file": "^4.5.0",
    "url": "^0.11.0",
    "google-auth-library": "^6.1.3",
    "pug": "^3.0.0",
    "mybatis-mapper": "^0.6.5",
    "moment" : "^2.29.1"
  }
}

```
