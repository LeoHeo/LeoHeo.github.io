---
layout: post
title: JPA ManyToMany 매핑 방법
category: Java
---

본문에 나오는 내용은 [자바 ORM 표준 JPA 프로그래밍](http://aladin.kr/p/8GfXs){:target="_blank"}에 내용을 참고했습니다.

## 다대다(N:N)
- 객체는 DBMS의 테이블과 다르게 객체 2개로 다대다(N:N)관계를 만들 수 있다.
- 회원(Member), 상품(Product)이라는 Entity가 있고 이 2개의 관계는 다대다 관계라고 예시를 들어보겠다.

{% gist 922f46abf32d58c27687010581550256 %}

Entity Class의 구조가 위와 같을때 **JPA는 아래와 같은 구조로 연결 테이블을 자동생성**해준다.

{% gist 223b2b4f6b9ea0997eb31cd4c341842a %}

그런데 생성되는 테이블 DDL을 보면 뭔가 컬럼명들이 눈에 거슬린다...
MEMBERS, PRODUCT가 두번이나 반복이 된다. 뭔가 깔끔하지가 않다.

## 좀 더 깔끔하게 연결 테이블 만들기
- @JoinTable을 이용하면 좀 더 깔끔하게 연결테이블을 만들 수 있다.
- JoinTable을 사용할때는 name, joinColumn, inverseJoinColumn을 지정해줘야 한다.
- Entity를 클래스를 아래와 같이 바꿔주면 DDL이 아까 보다 깔끔해졌다.

{% gist 238dccc25e5bb5a73f474d44c2834525 %}

## 자동생성 연결테이블의 한계
- 자동생성이 좋긴하지만 실제로 비즈니스를 하다보면 연결테이블에 MEMBER_ID, PRODUCT_ID만 사용하기에는 한계가 있다.
- 만약 요구상황이 연결 테이블에 주문 수량 컬럼(ORDERAMOUNT), 주문한 날짜 컬럼(ORDERDATE)까지 필요한 상황이 왔다고 해보자
- 이런경우에는 자동생성을 해주는 연결테이블을 더이상 사용을 할 수 가 없고 연결 엔티티를 따로 만들어줘야 한다.
- 그리고 Member와 Product도 다대다에서 일대다(1:N)와 다대일(N:1)로 풀어야 한다.
- MemberProduct라는 Entity를 만들어서 Member와 Product Entity를 연결해보자

{% gist a0be82e9f3a342e15314e6fde138fa08 %}

## 복합기본키와 식별관계
- MemberProduct는 MEMBER_ID와 PRODUCT_ID로 이루어진 복합기본키(복합키)로 이루어졌다.
- JPA에서 복합키를 이용할려면 별도의 식별자 클래스(MemberProductId)를 만들어줘야 한다.

```
복합키를 위한 식별자 클래스 특징
- Serializable을 구현해야 한다.
- equals와 hashcode를 메소드를 구현해야 한다.
- 기본생성자가 있어야 한다.
- 식별자 클래스는 public이어야 한다.
```

이렇게 복합키를 만들면 find을 할때 항상 식별자 클래스를 먼저 만들어줘야 한다.

{% gist 696da72b1ee45fb2c357521d9ee8ce63 %}

ORDERAMOUNT, ORDERDATE 추가하기 위해서 엄청난 작업이 들어간다.....
그리고 ORM 매핑시에도 처리할 일이 상당히 많아진다.

## 비식별관계로 좀 더 깔끔하게 연결 엔티티 만들기
- 복합키를 사용하지 않고 좀 더 깔끔하게 다대다 관계를 구성하는 방법을 알아보자
- 이 방법은 데이터베이스에서 자동으로 생성해주는 대리키를 Long값으로 사용하는것이다.
- 장점으로는 키가 비즈니스에 영향을 받지 않고, ORM 매핑시에 아까 같은 복합키를 만들 필요가 없어진다.
- 연결 테이블에 새로운 기본키를 사용하기 위해서 회원상품(MemberProduct)보다는 주문(Order)라는 이름의 Entity를 만든다.
- Order Entity는 더이상 @IdClass가 필요없다.

{% gist 5b346679e04b35e97993e1f15dc15844 %}

이렇게 식별자 클래스를 사용하지 않으면 find를 할때 한결 코드가 단순해진다.

{% gist 1863f62865c3f786acc4ba2e71e6be2c %}

## 결론
- 지금까지 JPA에서 다대다 관계 매핑에 대해서 살펴봤다.
- 다대다 관계를 일대일 다대일 관계로 풀어내기 위해 연결 테이블을 만들때 식별자를 어떻게 구성할지 선택해야 한다.
- 식별관계와 비식별관계중에 비식별 관계가 식별자 클래스를 만들지 않아도 되므로 단순하고 편리하게 ORM 매핑을 할 수 있다.




