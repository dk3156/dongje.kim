---
title: "자바에서 자주 보이는 Static 이란 무엇일까?"
date: 2023-11-03
mainSectionTitle: "hugo"
---
# JVM 메모리 구조

Java Virtual Machine 은 
1. Garabage Collector
2. Execution Engine
3. Class Loader
4. Runtime Data Area
4가지 영역으로 나누어지고 이중 Runtime Data Area에 스태틱 변수가 관련이 있다. 
데이터 에리어는 메서드, 힙, 스택, PC레지스터, Native 메서드 스택 5가지로 구분되는데 이 중 Method Area 에 static 변수가 있다. 로드된 후 메모리에 항상 상주하고 있는 영역이다. 

클래스로더가 static 키워드를 보는 순간 객체가 생성되지 않아도 항상 메모리를 할당해야 하는 멤버로 보고 Method Area 에 메모리를 할당한다. Static 키워드가 붙은 멤버들은 겍체에 소속된 변수가 아니라 클래스에 소속된 변수이기 때문에 클래스 변수 혹은 클래스 메서드라고도 부른다. new 를 통해 객체를 생성하면 각 인스턴스는 서로 독립적이지만 static 키워드가 붙은 멤버들은 모든 객체가 메모리 영역을 공유하기에 공통으로 같은 영역으로 바라보면 된다. 

static 은 메모리 낭비를 줄이고 속도를 증가시키는 역할을 한다. 