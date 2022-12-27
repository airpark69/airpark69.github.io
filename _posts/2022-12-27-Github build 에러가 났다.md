---
title : Github build 에러가 났다
date : 2022-12-27 21:10:00 +09:00
categories : [Github]
tags : [Github, build error] 
---


![githuberror1](https://user-images.githubusercontent.com/50907018/209665697-ae32582e-5ea1-485b-a6d3-ffbce6a699a5.png)


Github blog에 글을 올리고 여유롭게 누워있는데, 이메일로 뭔가 도착했다.

빌드가 실패했다는 소리였다.

급하게 확인해보니 저런 상태였는데, 뭔가 build 자체가 업데이트가 된 모양이다.

Gemfile에 들어가서 ruby 3.2에 맞춰서 nokogiri의 버젼을 수정해주면 끝...

![githuberror2](https://user-images.githubusercontent.com/50907018/209665707-cb6faad8-3ff5-43de-bbdb-665cf6a9f3bf.png)

???

최신버젼의 nokogiri가 ruby 3.2를 지원하지 않는다.

어떻게 해야 하는가?

## 해결책
---

github blog의 루트 폴더로 가서

.github\workflows 의 경로를 따라가면

거기에 pages-deploy.yml 파일이 있을 것이다.

```yml
- name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3 # reads from a '.ruby-version' or '.tools-version' file if 'ruby-version' is omitted
          bundler-cache: true
```


내부에서 Setup Ruby 부분의

ruby-version을 3에서 2로 바꿔준다.

```yml
- name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2 # reads from a '.ruby-version' or '.tools-version' file if 'ruby-version' is omitted
          bundler-cache: true
```

이후

정상적으로 빌드가 되는 것을 확인했다.
