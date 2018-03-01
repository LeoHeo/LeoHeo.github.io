---
layout: post
title: Java8 Supplier을 활용하여 Lazy Evaluation 사용하기
category: Java
---

[모던 자바 (자바8) 못다한 이야기 - 05 Supplier, The Master of Lazy Evaluation](https://youtu.be/7e7FCMFrwcg)를 보고 Lazy Evaluation에 대해서 정리를 한번 해본다.

{% gist 494da21fa0abe504b7396628f526c60f %}

## Supplier를 사용하면 3초만 걸리는 이유
기존에 Supplier를 사용하지 않은 경우에는 조건문을 만나기전에 `getVeryExpensiveValue()`을 호출하자마자 즉시 가지고 온다.
그렇기 때문에 조건(num >= 0)에 상관없이 무조건 `getVeryExpensiveValue()`를 실행한다.

하지만 Supplier에서는 조건문에 만족할 경우에만 `valueSupplier.get()`을 하기 때문에 필요할때만 `getVeryExpensiveValue()`을 호출한다.