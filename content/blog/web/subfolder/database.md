---
title: "데이터베이스"
date: 2023-12-01
mainSectionTitle: "hugo"
---
## 데이터베이스의 종류

### key-value DB
key-value 형태로 데이터를 저장합니다. 너무 간단해보여서 실용성을 없어보여지? 맞습니다. 그래서 서브용 디비로 많이 쓰입니다. 
특수한 형태의 key-value 데이터베이스 Redis 는 많이 쓰이는데, 레디스는 데이터를 하드디스크에 저장하지 않고 일차적으로 램에 먼저 저장합니다. 
램은 하드디스크보다 몇백배 빠르기 때문입니다. 그래서 사용자가 자주 쓰는 데이터는 레디스에 복사후 요청때 보여줍니다. 캐싱용 서브 디비로 활용합니다.

### Relational DB
데이터를 표 형태로 저장하고 싶다면 relational db. row, column 형태로 (마치 엑셀같이) 데이터를 저장합니다. 많은 데이터를 저장하기 위해서는 MySQL, PostgreSQL, SQLite, Orcale 등을 사용합니다. SQL 문법을 써서 사용합니다. 데이터 정규화를 많이 쓴는데, 데이터 중복이 되면 다른 테이블로 쪼개버리는 과정을 정규화라고 합니다. 이 정규화 때문에 데이터 출력 문법이 복잡해 질 수 있습니다. 입출력 속도보다 데이터 정확도가 더 중요할때 관계형 디비를 사용합니다. 

### Graph DB
데이터 사이의 관계를 표현하기 위해 그래프 디비를 쓸 수 있습니다. neo4j 가 대표적인 그래프 디비로,노드라는 걸 만들고 데이터를 저장하고 노드 사이의 관계를 정의할수 있습니다. Graph Query Language 를 사용해 저장하는데, 비행기 노선, SNS 친구 관계등을 그래프를 사용합니다.

### Document DB
자유로운 데이터 형태로 저장합니다. MongoDB 가 예시이고 컬렉션이라는 폴더 안에 도큐멘트라는 형식의 데이터를 저장합니다. 예를들어 JSOn 같은. 데이터 구조가 바뀌어도 에러가 안남 (착합니다.) 데이터의 중복 제거(정규화)를 하지 않아서 입출력 문법이 간단합니다. 데이터 분산 처리를 잘합니다. 일관성, 정확도가 중요하지 않으면 도큐멘트 디비를 씁니다.

### Column-Family Database
컬럼 형태의 데이터를 저장하는데 행마다. cassandra, Google Big Table 이 예시입ㄴ다. 표처럼 행, 컬럼으로 만드는데 컬럼은 행마다 달라도 상관없습니다. 정규화 없이 쓴는게 일반적입니다 복제, 데이터 분산처리를 잘합니다. 다만 데이터 분산이 너무 많으면 일관성이 떨어집니다. 데이터의 시간기록을 저장하는 데이터베이스는 column family db 를 사용합니다.

### Search Engine
검색용 인덱스를 저장하는 데이터 베이스로, 일반적으로는 인덱스 보관용으로 많이 쓰입니다. 빠른 검색을 도와주는 목차를 인덱스로 부르고, 검색 요청이 오면 인덱스를 사용해서 데이터를 빠르게 찾아줍니다. 평소에 쓰는 데이터베이스를 Search Engine DB 에 입력하면 인덱스를 생성해 빠르게 검색할 수 있게 해준다. 검색이 중요한 사이트 만들때 많이 사용합니다. 


## 정규화란?
### 제 1 정규화
한 칸에 하나의 데이터만 저장합니다. 
### 제 2 정규화
현재 테이블의 주제와 관련없는 컬람을 다른테이블로 빼는 작업. partial dependency 를 제거한 테이블. 
Composite Primary Key( 두개 이상의 primary key 역할을 하는 컬럼) 종속된 컬럼 (따까리) 을 제거하는 과정입니다. 이 따까리의 관계를 partial dependency 라고 합니다.
### 제 3 정규화
Primary Key 컬럼이 아닌 잔챙이 컬럼에 종속된 컬럼을 제거해서 다른 테이블로 만드는 과정을 말합니다. 따까리의 따까리를 다른 테이블로 정규화 해줍니다.