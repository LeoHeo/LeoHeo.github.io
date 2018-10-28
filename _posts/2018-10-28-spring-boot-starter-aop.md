---
layout: post
title: Spring boot에서 Annotation 기반의 AOP
category: Java
---

예를들어 method의 총 실행시간을 측정을 하기 위해서는 어떻게 해야할까?
단순하게 생각하면 아래와 같은 코드를 기존코드에 넣어야 할것이다.

{% gist e27f769085cec68bce37dab515c1e79f %}

근데 위와 같은 코드를 넣어야 할 곳이 여러곳이라면 어떻게 하겠는가?

매번 method의 처음과 끝에 저 코드를 넣어야 할 것이다.

[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 라고 뭔가 중복되는 코드가 들어간다.

[Code smell](https://en.wikipedia.org/wiki/Code_smell) 나는 코드이다.

그렇다면 이걸 어떻게 개선할 수 있을까?

[AOP(Aspect-oriented programming)](https://en.wikipedia.org/wiki/Aspect-oriented_programming) 로 이걸 해결할 수 있다.

AOP의 정의에 대해서는 Wiki를 참고하길 바란다.

Spring boot에서 AOP를 사용하기 위해서는 gradle에 Dependency를 추가해줘야 한다.

Version 명시를 안해주는건 Spring boot에서 버젼관리를 해주기 때문에 굳이 명시할 필요가 없기 때문이다.

```
compile('org.springframework.boot:spring-boot-starter-aop')
```

인제 timeCheck 하던 부분을 Annotation 기반으로 만들기 위해서 Annotaion Class, Aspect class를 만들어준다.

{% gist db1a6cc9a0106f3a099cd2d494c1b904 %}