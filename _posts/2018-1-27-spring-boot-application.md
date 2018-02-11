---
layout: post
title: SpringBootApplication랑 SpringBootTest는 필히 root package에 위치해야한다.
category: Java
---

## 문제가 되는 경우
- SpringBootApplication이 `root에 없는 경우` Test케이스를 짤때나 InitializingBean으로 초기 데이터를 넣을때 문제가 된다.
- `No beans of 'FooRepository' not found` 에러가 뜬다.

## 해결방법
- `@SpringBootApplication`랑 `@SpringBootTest`은 `꼭 root-package`에 있어야 한다.
- ~~Spring-boot 공부하다가 이거 때문에 1시간 삽질했네....~~

{% gist c5831a0b692abed44e6f49da27e5982a %}
