---
layout: post
title: 자바 ORM 표준 JPA 프로그래밍 예제 h2 db tcp일 때 오류해결 방법
---

- 책에 나오는 h2 db version `1.4.187`이다.
- javax.persistence.jdbc.url -> `jdbc:h2:mem:test`로 할때는 아무 문제가 없다.

## 문제 발생
{% gist deaf8f578e04f5c243997f063dacf11c %}
- 위와 같이 설정했을때 `187` 버젼은 **Connection is broken**이 뜬다.

## 해결 방법
- h2 db version을 `1.4.187` -> **1.4.196** 으로 올린다.
- 언제 그랬냐는듯이 tcp연결도 아주 잘 된다.