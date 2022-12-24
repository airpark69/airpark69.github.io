---
title : Docker Compose로 Container 2개 이상 같이 사용하기
date : 2022-12-24 11:30:00 +09:00
categories : [머신러닝, 파이썬]
tags : [Docker, Docker Compose] 
---

# Docker Compose
---
도커에서는 컨테이너를 2개 이상 사용하여 기능을 활용하는 형태를 지원하고 있다.

이게 Compose이며, 사용 방법은 간단하다.


## 사용 방법
---
1. docker-compose.yml 을 만든다.
2. docker compose 공식문서를 활용하여 yml을 만든다.
3. CLI에서 docker-compose 명령어를 실행시킨다.



## 상세 내용
---
[공식문서](https://docs.docker.com/compose/compose-file/) 에서 더 자세한 내용이 있다.

```yml
version: '3.7'

services: 
  FrontEnd:
    # 웹 서버
    image: 프론트로 사용할 이미지(보통 웹쪽)
    ports:
      - "프론트 외부포트:내부포트"
    container_name: 컨테이너 이름
  BackEnd:
    image: 백엔드로 사용할 이미지
    container_name: 컨테이너 이름
```

간단히 이렇게 사용하면, 2개의 컨테이너를 묶어서 사용할 수 있다.

이후 해당 파일이 있는 위치까지 CLI 내부에서 이동한다

ls 명령어를 통해서 docker-compose.yml 파일이 존재하는 것을 확인 했으면 아래의 명령어를 실행한다.

```docker
docker-compose up -d
```
up은 생성 및 실행이며

-d 는 백그라운드 실행을 의미한다.

[compose 공식문서](https://docs.docker.com/compose/reference/) 에서 더 많은 옵션을 확인할 수 있다.



