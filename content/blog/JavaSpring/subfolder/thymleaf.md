---
title: "Thymleaf"
date: 2023-11-04
mainSectionTitle: "hugo"
---
# Thymleaf
thymeleaf는 Spring Boot에서 권장하는 JSP를 대체하는 서버사이드 자바 템플릿 엔진입니다. Spring Boot에서는 JSP보다 더 간단한 설정과 HTML 표준 문법으로 thymeleaf를 이용해서 HTML을 작성할 수 있습니다.

도큐멘테이션 https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html  참고.

### 태그
* "$" -- 컨트롤러에서 전달해주는 변수를 사용.
* "*" -- selection 변수. 오브젝트의 어트뷰트 값을 가져와 사용.
* "#" -- 메시지 값을 가져와 사용.
* "@" -- 링크 값을 가져와 사용.
* "~" -- 프래그먼트 값을 가져와사용.
프래그먼트란 코드 조각을 의미하는데, html 코드 조각을 가져와 붙여넣거나 기존 코드와 대체시킬 수 있다.

string literal, int, null, + - * %, and, or, <= , <, > , if, if else 문 전부 태그 안에 들어갈 수 있다.

예시: 
```
<p th:text=”#{home.welcome}”> hello world </p>
```
hello world 텍스트가 home.welcome의 내용으로 대체된다.
```
<link href=”../css/example.css” th:href=”@{css/example.css}”/>
```
로컬 환경에서는 앞의 href 가 참고가 되지만 실제 서버를 구동하면 th가 붙어있는 커맨드로 대체된다.
```
<th: object = “{${object}}>
<th:text=”*{firstname}”>
```
$ 태그로 받은 오브젝트 값을 * 태그를 통해 엑세스하고 그 오브젝트의 어트리뷰트를 사용한다.
```
<th:if=”${prod.count} &gt; 1”>
```
if 문의 예시로 prod.count 가 1 보다 크다면 다음 코드가 실행된다.
```
<th:text=”mode is” + ((${exemode} == ‘dev’) ? ‘development’ : ‘production’)”>
<th:class=”${row.even}? ‘even’ : ‘odd’ “>
```
ternary exprerssion의 예시이다.
```
<input type=”checkbox” name=”active” th:checked=”${user.active}” />
```
user object 의 active 값이 참이면 체크박스가 체크가 되고 부정이면 체크되지 않는다.
```
<div th:fragment = “copy”>
	blah blah
</div>
…
<div th:insert=”~{footer :: copy} > </div>
```
* insert 태그가 들어간 div 에 blah blah 텍스트가 들어간 div가 더해진다.
* th:insert -- 태그 안에 카피한 내용을 더한다.
* th:replace -- 카피한 내용으로 태그 자체를 대체한다.

### 조건문
```
<th:each=”prod : ${prods}”>
	<th:text=”${prod.name}”> onions </>
	<th:text=”${prod.instock}? #{true} : #{false}”> yes </>
```
for each 문의 예시이다.
```
<th:swich=”${user.role}”>
	<th:case=”’admin’”>
	<th:case=”#{role.manager}”
```
switch, case문의 예시이다

### 유틸리티
```
th:if=”${not #lists.isEmpty(prod.comments)}”
```
"#"기호를 통해 라이브러리를 사용할 수 있다. 예제에서 lists.isEmpty 유틸리티를 써서 if 문을 작성했다.

### 블록태그
```
<th;block th:each=”user: ${users}”>
	<th;text=user.login>
```
블록 태그는 제 역할을 하지만 실제로 보이지는 않는다. 

### expression inlining
```
<p> the message is “[(${msg})]” < /p>
<p> hello, [[${session.name}]] </p>
```

### javascript inlining
```
<script th:inline=”javascript”>
 var username = [[${session.user.name}]];
```