---
title: "MySQL"
date: 2023-12-06
mainSectionTitle: "hugo"
---
## 1주차 링크
https://teamsparta.notion.site/SQL-1-bba7788fb6b54b609e389c438678ba0e

## 2주차 링크
https://teamsparta.notion.site/SQL-2-0f26aba347204efbbcd9c6db432a15d5

중요개념!:

### select, from, where, order by, group by, min, max, avg, count,
### like '%%',
### in (value, value ...),
### inner, outer, left, right join, 
### =, <> , >=, <=
### group by - 1, 2, 3 : select 한 첫번째, 두번째, 세번째 컬럼을 기준으로 그룹한다.

## 3주차 링크 
다른 개발자분의 말...
```
데이터베이스를 설명하려고 하면 한도 끝도 없고 배울 내용도 하루 이틀에 끝날 내용이 아니다.
데이터베이스는 너무나 방대하고 데이터베이스가 없으면 프로그래밍이 아무런 소용이 없다.
데이터베이스가 프로그래머의 기본소양이라고 하는데 그 기본 소양이라고 하는 것이 사실 프로그램의 공부 영역을 아득히 앞서 나간다는 것이 함정이기는 하지만 사실 본인이 필요했던 것은 기본과 초심이었다.
그러한 측며에서 부트캠프를 다니고 있다는 것이 마냥 비효율적이라던가 불합리하다고 전혀 생각하지 않는다.
오히려 절호의 기회이고 모르는 것을 알고 되짚어 갈 수 있는 기회가 될 수 있다라고 보고 있다.
데이터베이스 분야 관련 뿐만 아니라 다른 리눅스라던가 파이써이라던가 다른 여러가지 기초부터 차근차근 밟아 나가다보면
지난 5년간 바쁘게 걷기만 하다 잃어버렸던 나의 길을 다시 되찾을 수 있을 곳이라 생각하고 있다.
```
https://teamsparta.notion.site/SQL-3-efd9ca0308a341bc8b3cba2348f06a08

중요개념!

## replace(column, 'chars to chage', 'chars for change')

## substr(column, start_index, end_index)

end_index 가 스트링의 끝까지 포함하려면 생략한다.

## concat(substr1, substr2, substr3...)
여러 컬럼을 합칩니다.

데이터 정규화, 페이지네이션, 인덱싱, 캐시, 메모리, 디스크, Acid, Transaction, 버퍼메모리, 캐시메모리, 
속도향상!

## If, Case
조건에 따라 연산을 적용할 수 있습니다.
```
if(조건, 조건을 충족할때, 조건을 충족하지 않을때)

case when 조건1 then 값(수식)1
     when 조건2 then 값(수식)2
     else 값(수식) 3
end
```
두 조건 다 임의로 새로운 컬럼을 만드는 명령어입니다.

user-segmentation 이란?

## 4주차 링크

https://teamsparta.notion.site/SQL-4-489d8f54a96746c8a328589a2861d3a4

## cast (as decimal, as char) 함수
문자를 숫자로, 숫자를 문자로 변경해주는 함수.
```
--숫자로 변경
cast(if(rating='Not given', '1', rating) as decimal)

--문자로 변경
concat(restaurant_name, '-', cast(order_id as char))
```

## Subquery
필요한 경우
1. 여러번의 연산을 수행할때.
예를 들어서, 수수료 부과, 시간에 따른 주문 금액 계산, 금액에 따흔 가중치 계산, 가중치에 따른 최종 배달비 계산
2. 조건문에 연산 결과를 사용할 때.
예를 들어서, 음식 타입별 평균 음식 주문금액에 따라 음식비 상/중/하로 나눌때. 타입별 평균 음식을 구하고 case when 문으로 상/중/하로 나눌때.
3. 조건에 쿼리 결과를 사용할 때.
조건문의 조건이 지금 사용하는 쿼리 결과에 따라서 달라질때
subquery 란 걸 쓸수 있다 정도만 기억해두기.

(a*b) * 2 와 같은 형식으로 괄호 안의 결과를 다시 사용하는게 subquery 문이다. 한번 쿼리를 해준다음에 그 결과물을 다시 사용하고 싶을때 쓴다!

## Join
left join - 공통 컬럼을 가지고 왼쪽 테이블 기준으로 합치는것. 오른쪽 테이블에 없는 데이터라도 왼쪽 테이블에 존재하면 조회가 가능하다.
right join - 공통 컬럼을 가지고 오른쪽 테이블 기존으로 합치는 것. 왼쪽 테이블에 없는 데이터라도 오른쪽 테이블에 존재하면 조회가 가능하다. 
inner join - 공통 컬럼을 가지고 두 테이블 모두 존재하는 데이터만 가지고 합치는 것. 
```
from table1 a left join table2 b on a.column = b.column
```
공통 컬럼의 이름이 달라도 상관이 없다. 공통된 값만을 보고 합치기 때문에 이름이 다르더라도 on a = b 구문에 명시해주면 이 컬럼을 기준으로 합쳐진다.

## 5주차 링크

https://teamsparta.notion.site/SQL-5-ef83acb4ed234cd3bbfe66dd46144303

테이블에 없는 데이터, 또는 잘못된 데이터가 들어가 있을때는?
1. 없는 값을 제외해주기
```
select a.order_id,
       b.name
from food_orders a left join customers b on a.customer_id = b.customer_id
where b.customer_id is not null
```
a 에는 있는 customer_id 가 b 에는 없을 때, null 로 처리가 될까? (추측)
그래서 b 의 customer_id 가 null 일 경우에만 제거한다면, a 와 b 같이 포함하는 데이터만 가져오는 것이다. 
위 코드는 inner join 과 같은 결과가 나온다.

2. 다른 값으로 대신해주기
평균값, 또는 중앙값을 이용해 대체해준다.
두개의 문법을 이용할 수 있다. 둘다 기존 컬럼을 수정하는게 아니라 새로운 컬럼을 만들어 낸다.
* 다른 값이 있을 때 조건문 이용 : if(rating > 1, rating, 대체값)
* null 값일때: coalesce(age, 대체값)
```
select a.order_id,
       coalesce(b.age, 20) "null 제거",
       b.gender
from food_orders a left join customers b on a.customer_id=b.customer_id
where b.age is null
```

상식적이지 않은 값이라면?
* 예를들어, 주문 손님들의 나이는 보통 20세 이상이어여 하는데, 2세 데이터가 존재한다.
*  결제 일자가 1970 년대이다.

조건문으로 값의 범위를 지정한다. (상식적인 수준의 범위를 지정해 준다.)
값이 상식적인 수준 범위를 넘어서면 다른 값으로 대채한다.
```
select customer_id, name, email, gender, age,
       case when age < 15 then 15
            when age > 80 then 80
            else age end  "범위 지정된 나이"
from customers
```

## pivot view
Pivot table 이란? : 2개 이상의 기준으로 데이터를 집계할 때, 보기 쉽게 배열하여 보여주는 것을 의미

성별, 연령별 주문건수 Pivot Table 뷰 만들기  (나이는 10~59세 사이, 연령 순으로 내림차순)
1. 성별, 연령별 주문건수 집계하기 
```
select b.gender,
       case when age between 10 and 19 then 10
            when age between 20 and 29 then 20
            when age between 30 and 39 then 30
            when age between 40 and 49 then 40
            when age between 50 and 59 then 50 end age,
       count(1)
from food_orders a inner join customers b on a.customer_id=b.customer_id
where b.age between 10 and 59
group by 1, 2
```

2. Pivot view 구조 만들기
```
select age,
       max(if(gender='male', order_count, 0)) male,
       max(if(gender='female', order_count, 0)) female
from 
(
select b.gender,
       case when age between 10 and 19 then 10
            when age between 20 and 29 then 20
            when age between 30 and 39 then 30
            when age between 40 and 49 then 40
            when age between 50 and 59 then 50 end age,
       count(1) order_count
from food_orders a inner join customers b on a.customer_id=b.customer_id
where b.age between 10 and 59
group by 1, 2
) t
group by 1
order by age
```

두개의 데이터로 pivot view 만들때, 테이블에서 그 두개의 데이터만 뽑아온 테이블을 하나 만들고, 그 테이블을 pivot vieew 형식으로 재배열하는 방식으로 만든다. 원하는 컬럼 뽑기 -> pivot view 의 중심이 되는 세로 컬럼을 기준으로 정렬 (order by) -> pivot view 의 중심이 되는 가로 컬럼을 만들어준다. 이때 max(if()) 함수를 쓰는데 max() 함수를 쓰는 이유는 무엇인지 모르겠다.

