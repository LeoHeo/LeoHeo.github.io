---
layout: post
title: AWS Reserved Instance
category: AWS
---

## 제공 클래스
- 표준 RI(standard) -> on-demand보다 40%정도 할인되며 가장 싸다. 다만 `인스턴스 타입 변경 불가능` -> t2.large 썼으면 쭉 t2.large
- 컨버터블 RI(convertible) -> on-demand보다 30%정도 할인 장점으로는 `기존 인스턴스보다 성능 높이는거 가능` -> t2.large 쓰다가 m시리즈처럼 높이는것만 가능

## 인스턴스 1대 1년 기준 (e.g t2.large)
- 전체 선결제(All Upfront) -> 전체 선결제($563)를 하고 나머지 금액을 `안낸다`
- 부분 선결제(Paritial Upfront) -> 특정금액을 `계약금처럼 먼저 내고($330)` 매달 할인된 금액(`0.038 * 720 => 27.36`)을 낸다.
- 선 결제 없음(No Upfront) -> on-demand보다는 싸지만 1년동안만 사용할 수 있고 그 후로는 사용할 수 없다.

## 기타
- 대수가 많으면 같은 계열내에서는 2대 합쳐서 상위 1대로 가거나 반대로 1대를 하위 2대로 분리도 된다.
- m4.2xlarge 1대 -> m4.xlarge 2대
- 단, 기간은 같아야 한다.