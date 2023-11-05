---
title: "JPA 이란?"
date: 2023-11-03
mainSectionTitle: "hugo"
---
https://yjkim-dev.tistory.com/3
https://dbjh.tistory.com/77 참고. 

# JPA and H2

H2 Database 란?
H2는 자바로 작성된 관계형 데이터베이스 관리 시스템이다. JAVA로 작성된 오픈소스 RDBMS 이며 스프링 부트가 지원하는 인메모리 관계형 데이터베이스이다.
인메모리로 띄우면 애플리케이션 재기동 때마다 초기화된다. 로컬 환경, 테스트 환경에서 많이 쓰인다. 


장점- 
따로 설치가 필요없다
용량이 매우 가볍다
웹용 콘솔(쿼리툴) 제공하여 개발용 로컬DB로 사용 용이
특징

JPA 란? 
Java 진영에서 ORM 기술 표준으로 사용하는 인터페이스 모음. 자바 어플리케이션에서 관계형데이터베이스를 사용하는 방식을 정의한다. 

ORM 이란?
어플리케이션 클래스와 관계형 데이터베이스의 테이블을 매핑한다는 뜻이다. 

JPA는 반복적인 CRUD SQL을 처리해준다. JPA 는 매핑된 관계를 이용해서 SQL을 실행하고 생성하는데, 개발자는 어떤 SQL이 실행될지 생각만하면 되고, 예측도 쉽게 할 수 있다. 추가저으로는 JPA는 네이비트 SQL이란 기능을 제공해주는데, 관계 매핑이 어렵거나 성능에 대한 이슈가 우려되는 경우 SQL을 직접 작성하여 사용할 수 있다. 

JPA를 사용하면 SQL이 아닌 객체 중심으로 개발할 수 있다는 것이다. 이에 따라 생산성이 좋아지고 유지보수도 수월해 진다. 

### @ID and @GeneratedValue

@Id 어노테이션은 식별자 필드로, 엔티티의 필드를 테이블의 기본 키 (PK, Primary key)에 매핑하는 역할을 한다.

@GeneratedValue 어노테이션은 어노테이션명처럼 자동 생성을 도와준다. 즉 primary 키의 값을 위한 자동 생성 전략을 명시하는데 사용한다

### 자바 인터페이스란?
자바 인테페이스(오오피 상속, 추상클래스 느낌)

인터페이스(interface)란 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스를 의미합니다.

인터페이스는 추상 클래스와 마찬가지로 자신이 직접 인스턴스를 생성할 수는 없습니다. 따라서 인터페이스가 포함하고 있는 추상 메소드를 구현해 줄 클래스를 작성해야만 합니다.

https://gofnrk.tistory.com/22 참고.

### JPA Repository

실제 DB에 Access 하여 쿼리를 수행하는 등의 역할을 하는 Repository Interface를 생성합니다. @Repository 어노테이션을 추가하고, JpaRepository를 상속합니다. JpaRepository는 Spring Data JPA에서 제공하는 JPA 구현을 위한 인터페이스로 간단하게 상속하여 사전에 정의된 여러 메서드를 통해 간단히 DB에 Create/Read/Update/Delete 쿼리를 수행할 수 있습니다

예시:``` java interface extends JPArepository<class name>```