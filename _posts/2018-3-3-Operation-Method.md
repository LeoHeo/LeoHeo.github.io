---
layout: post
title: Stream Operation Method에 대해서 알아보자(Intermediate, Terminal)
category: Java
---

{% gist a6132c574cddd85038b8d48cfda07dc3 %}

## Intermedidate Operation Method
- Stream을 리턴하는 Method
- Intermedidate Operation Method끼리는 chaining이 가능하다.

## Terminal Operation Method
- Operation 끝내는 Method
- Terminal Operation Method가 호출되면 그전까지 있었던 Intermedidate Operation Method 연산들을 한다.