---
title : docker metabase - flask - postgreSQL 연동한 compose
date : 2023-01-08 23:47:00 +09:00
categories : [머신러닝, 파이썬]
tags : [docker, docker error] 
---
# Metabase

---

- BI(Business Intelligence) 툴

- 데이터의 시각화를 도와준다.

- DB와의 실시간 연동으로 Dashboard를 만들 수 있다는 것이 강점


# Flask

---
- Django와 같이, 팩토리 패턴으로 WEB 애플리케이션을 구축하도록 도와주는 패키지

- 간단한 기능, 작은 규모의 프로젝트에 적합

- flask_sqlalchemy 라는 추가 패키지로 postgreSQL과  쉽게 연동 가능하다


# postgreSQL

---
- 객체-관계형 데이터베이스 관리 시스템

- CRUD 기능이 MySQL보다는 늦지만, 복잡한 쿼리일수록 효율이 좋아진다.




## Docker-compose

---

도커에서의 연동이 어려운건 아니지만 주의할 점은 환경 변수다.

환경 변수를 보통 .env 같은 파일로 관리하거나 secret 파일에 넣곤 하는데

이 때, 제대로 환경 변수가 들어갔는지 Docker desktop에서 실행 중인 container에 들어가서 inspect로 확인해주는게 좋다.

오류가 발생할 경우, 환경 변수를 최우선으로 확인한다.


```yml
version: "3.9"

services:
    web:
        build:
            context: .
        restart: always

        env_file:
            - .env
        ports:
            - 5000:5000
        environment:
            - FLASK_APP=./src/run.py
        networks:
        - metanet1-secrets
        volumes:
            - ./:/app
        depends_on:
            - db
    db:
        container_name: postgres
        hostname: postgres
        image: postgres:latest
        ports:
            - 5432:5432
        environment:
            - POSTGRES_DB=${PG_DB}
            - POSTGRES_USER=${PG_USER}
            - POSTGRES_PASSWORD=${PG_PASSWORD}
        networks:
        - metanet1-secrets
        volumes:
            - db_data:/var/lib/postgresql/data
    pgadmin:
        container_name: pgadmin
        image: dpage/pgadmin4
        ports:
            - 8088:80
        environment:
            - PGADMIN_DEFAULT_EMAIL=${PG_ADMIN_EMAIL}
            - PGADMIN_DEFAULT_PASSWORD=${PGADMIN_DEFAULT_PASSWORD}
        networks:
        - metanet1-secrets
    
    dataviz:
        image: metabase/metabase:latest
        container_name: metabase
        volumes:
        - dataviz_data:/metabase-data
        ports:
        - 3000:3000
        environment:
            - MB_DB_TYPE=postgres
            - MB_DB_DBNAME=${PG_DB}
            - MB_DB_PORT=${PG_PORT}
            - MB_DB_USER=${PG_USER}
            - MB_DB_PASS=${PG_PASSWORD}
            - MB_DB_HOST=postgres
            - MB_DB_FILE=/metabase-data/metabase.db
        networks:
        - metanet1-secrets
        depends_on:
        - db
networks:
  metanet1-secrets:
    driver: bridge
volumes:
    db_data:
    dataviz_data:
```


- 설정에 대해서 세부 사항은 프로젝트에 맞게 바꿔주면 된다.

- 기본적으로 .env를 사용한다는 환경

- ${변수이름} 부분은 환경변수를 의미한다.


---

flask로 구현한 파이썬 web 프로젝트를 dockerfile을 통해 빌드하고, DB와 BI툴을 compose하면 서비스의 기본적인 부분이 완성된다.

추가적인 기능에 대해서는 추가할 때, 찾아보는 편이 좋다.

### 주의할 점

-  오류가 뜨면 하나하나 고쳐본다.
