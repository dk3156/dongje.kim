---
title: "AngularJS, 양방향 바인딩이란?"
date: 2023-11-03
mainSectionTitle: "hugo"
---

# AngularJS 에서 양방향 데이터바인딩이란?

<input type="text" ng-model="menu.itemCount"> 
menu 객체의 itemCount 속성을 텍스트 입력박스의 값으로 바인딩 해주는 코드. 
일일히 input 값을 처리하는 함수를 만들지 않아도 된다. 양방향 데이터 바인딩은 AngulaJS 의 주요 기능이라고.

양방향 바인딩이란, 데이터 변화를 감지해 템플릿과 결합하여 화면을 생성하는 것. 양방향의 장점은, 코드량을 줄여준다는 것이다. 단점은 변화에 따라서 DOM 객체 전체를 렌더링해주거나 데이터를 바꿔줘야 해서 성능이 감소되는 경우이다.

단방향의 장점은, 반대로 성능저하 없이 DOM객체 갱신이 가능하고 데이터 흐름이 단방향이라 이해가 쉽고 디버깅이 쉬울 수 있다. 단점은 변화를 감지해서 업데이트하는 코드를 매번 작성해야한다. 