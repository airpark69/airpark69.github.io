---
title : docker image build, compose
date : 2023-01-05 23:00:00 +09:00
categories : [머신러닝, 파이썬]
tags : [docker, docker error] 
---

# image build
---

docker를 활용하여 형상관리를 위해서는 image를 빌드하여 하나의 가상 환경을 만들어줘야 한다.

이를 위해 프로젝트를 하면서 마주한 부분을 적어본다.

## 필요사항
---

- dockerfile 
-> 확장자 없이, 그냥 dockerfile 이라는 이름으로 지정해준다.
dockerfile을 작성하는 규칙이 있으므로 공식문서 등을 활용하여 작성하도록 한다.

이때, 본인이 image를 빌드할 환경에 따라 사소하게 명령어의 차이가 있다는 점을 명심하자

- .env
-> 환경변수를 지정해주는 파일, 꼭 필요하진 않지만 관리를 위해서 사용하는 편이 좋다

- docker-compose.yml
-> image를 여러개 합쳐서 컨테이너를 만들기 위한 설정 파일으로, WEB, DB처럼 여러 기능이 합쳐져야 할 경우 사용한다.


### 명령어 

- image만 빌드할 경우
```docker
docker build -t project .
```

프로젝트 root 폴더에서 cmd나 git bash 등에서 명령어를 사용하면 빌드가 시작된다.

---

- compose를 활용할 경우
```docker
docker-compose up -d 
```

WEB, DB 등의 이미지를 compose할 경우

이 때, docker-compose.yml에서 포트 설정과  환경 변수 부분을 주의해서 설정해야 한다.



## 왜 docker?
---

분명 처음에는 docker에서의 어려움을 마주할 수 있다.

그러나, docker 자체의 문법이나 사용법만 숙지하고 절차적인 오류를 해결하고 나면 docker를 통해서 실질적으로 개발 이후 배포에 대해서 문제점을 해결하기 쉬워진다.

결국 조금 돌아가는 방법이지만, 길게 봐서 향후의 문제를 예방하는 차원이라고 생각한다.

