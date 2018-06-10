---
layout: post
title: Java의 Equals와 Hashcode
category: Java
---

## 기초를 정리해보자
- 최근에 Java에서 class를 정의하고 왜 `equals()`, `hashCode()`를 override 해야 하냐? 라는 질문을 받아서 그 당시에 구두로 이래저래서 필요하다고 답변을 했는데 오늘 집 앞 카페에서 공부하면서 한번 정리를 해야겠다고 생각해서 정리해본다.

## Identity and Equality
- `서로 다른 Instance를 같은지 다른지 어떻게 비교할까요?` 라고 Java 기술면접에서도 가끔 나오는 문제이기도 한다.
- 이걸 말할때 항상 나오는 2가지가 `Identity`, `Equality` 이다.

```
Java에서 Identity와 Equality
hashcode - Identity(동일성)
equals - Equality(동등성)
```

## 같은 학생인지 아닌지 비교를 해보자.

{% gist e769b1e651ca43f4531868d7fe4500ce %}

- `Leo1`, `Leo2` 라는 인스턴스를 만들어서 `equals`를 비교했는데 `false`가 나왔다.
- `Leo1`, `Leo2` 이 같은지 다른지 판단하기 위한 기준을 정해보자.
- 여기서는 id가 같으면 서로 같은 학생이라고 판단하고 `equals()`를 Overriding 해보자

{% gist a1327a68a5eb2a70346ddfb9bdd0a3c5 %}

위와 같이 `true`가 나오는걸 확인 할 수 있다.

`equals`를 재정의 하게 되면 ArrayList에서도 아래와 같이 사용할 수 있다.

{% gist f597fd6b320c7bad9ab49a9151732484 %}

## hashcode의 쓰임새란?
- 지금까지 살펴보니 equals만 override를 해도 될거 같은데 hascode는 왜 재정의를 해야할까요?
- 필요성을 느끼기 위해서는 HashMap에 대해서 살펴볼 필요가 있습니다.

{% gist 25bbdfd7197589879e64c44fc8ed8786 %}

Java에서 HashSet을 설명할때 나오는게 `A collection that contains no duplicate elements`이라는 이야기가 나옵니다.
하지만 위에 코드를 보게 되면 id가 같은 Leo라는 Student가 중복해서 들어갔습니다.

![](https://pbs.twimg.com/media/B_qA91VUsAEmjU3.jpg)

~~아니 이게 무슨소리요...중복이라니...중복이라니...~~

왜 중복해서 들어갔는지 알기 위해서 HashSet의 Core소스를 한번 살펴보겠습니다.

HashSet을 생성하게 되면 내부적으로는 HashMap을 생성하게 됩니다.
그래서 add를 할때마다 `map.put()`을 하게 됩니다.

{% gist d8dbcb11caac8441a898ae3234722ff8 %}

그럼 HashMap은 put을 할때 어떤식으로 put을 할까요?

{% gist 88e42e2b1cc04a317e552ab887ad8ff9 %}

HashMap put의 작동 방식은 해당 Object의 hashCode를 가져와서 중복이면 그걸 대체하고 중복이 아니면 넣는 구조입니다.

그럼 인제 `hashcode`를 재정의를 해보고 정말로 중복이 제거가 되는지 한번 알아보겠습니다.

{% gist 503644e401bc0c71eee61bf3578e0101 %}

hashCode를 재정의 하니깐 정말로 중복이 제거가 되어서 Size가 1이 나옵니다.

## Conclusion
- java에서 `equals()`, `hashcode`에 대해서 알아보고 왜 override 해야 하는지 내부 구현을 살펴보면서 알게되었습니다.
- HashSet, HashMap의 내부 구현을 살펴볼 수 있었던 좋은 기회였습니다.

## Reference
- [Working With hashcode() and equals()](https://dzone.com/articles/working-with-hashcode-and-equals-in-java)