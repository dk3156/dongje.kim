---
title: "자바 스프링 튜토리얼"
date: 2023-10-30
mainSectionTitle: "hugo"
---
https://velog.io/@kjyeon1101/Spring-Controller%EC%99%80-Service 참고. 

### 데이터 베이스 테이블과 자바 클래스를 매핑하는법
spring-boot-starter-data-jpa 의존성을 추가하고 @Entity 어노테이션을 붙여서 매핑한다.

### @RequestBody, @ResponseBody
JSON 형식의 데이터를 받기 위해 클라이언트에서 요청할 때 JSON -> Java Object 변환이 필요하다. 서버에서도 Java Object -> JSON 변환해서 보내줘야 한다. 

@ResponseBody -> 자바 객체를 JSON 내용으로 매핑한다. return type에 맡는 message Converter 를 통해 리턴하는 객체를 해당 타입으로 변환해서 클라이언트로 전달한다. 

```
request body = 
{
    "id":"test0101"
    "email":"test@gmail.com"
}

public class Member {
    private String id;
    private String email;
}
```
이와 같이 바디를 클래스 객체로 변환 시킨다.

### @RestController
http 관련된 코드 밑 요청/응답 매핑을 스프링이 알아서 해준다. @Controller + @ResponseBody 가 합쳐진 형태로 JSON 형태의 객체 데이터를 반환한다.

클라이언트에서 리소스 get 요청 -> 자바 서버에서 requestMapping 이 페이지 주소를 http 메서드와 매핑한다. 

### @GetMapping
Get 요청 방식의 API를 만들때, @RequestMapping(method = RequestMethod.GET) 방식도 있지만, @GetMapping 을 이용하는 방법도 있다. 아래 예시를 보자.
```
@RestController
@RestMapping("api")
public class GetController {
    ...
    @GetMapping("/getParam")
    public String getParameter(@RequestParam String id, @RequestParam(name = "password") String pwd){
        return "ID:" + id + ",Password:" + pwd;
    }
}
```
기본적으로 인자 변수명을 파라미터명으로 받는다. 하지만, @RequestParam(name = "원하는 파라미터 명")을 이용하면, 파라미터명을 바꿀 수 있다.

주소가 /api/getParam?id=블라블라 일때@RequestParam String id 의 값은 블라블라이다.

주소가 /api/getParam?password=블라블라 일때 @RequestParam(name = "password") String pwd 의 값은 블라블라.

파라미터가 많아진다면 VO (value object) 객체를 생성하여 받을 수도 있다. 우선 VO 객체를 생성한다. 그래고 생성한 객체를 아래와 같이 파라미터값으로 넣어준다.
```
@GetMapping("/getMultiParam")
public String getMultiParameter(VO_name vo){
    return vo.toString();
}
```

위에서는 계속 문자열을 반환했지만, 요즘 API는 대부분 JSON 형식을 사용한다.
```
@GetMapping("/returnJson")
public SearchParamVO returnJson (SearchParamVO vo){
    return vo;
    //{"username": "tester",
        "email":"tester@google.com",
        "page":1}
}
```
@RestController 를 이용하면, 내부적으로 Jackson 라이브러리에 의해 VO 객체를 JSON 형태로 변환하여 응답해준다. 만약 @RestController가 아닌 @Controller를 사용한다면, @ResponseBody를 추가해줘야한다.

### 예제 - Spring 으로 간단한 RESTful 웹 서비스 만들기

Greeting 이라는 리소스 표현 클래스를 만들어서 id 와 클래스 데이터를 view 에 렌더링하는 것을 목적으로 한다.
```
package com.example.restservice;
public record Greeting(long id, String content){}
```
Jackson JSON 라이브러리를 사용해 객체를 JSON 으로 자동 매핑해준다. 

다음으로 리소스 컨트롤러를 생성한다. 웹 서비스 http 요청은 컨트롤러에 의해 처리된다. @RestController 어노테이션은 이와같은 컨트롤러를 지칭. 
```
@RestController
public class GreetingController{
    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", defaultValue = "World) String name){
        return new Greeting(counter.incrementAndGet(), String.format(template, name));
    }
}
```
AtomicLong() 클래스는 Long 타입의 객체를 생성한다. 여기서는 id를 만들기 위해 씀.

전통적인 MVC 컨트롤러와 Restful web service 컨트롤러의 차이는 HTTP 응답 바디가 생성되는 방식에 있다. 서버 사이드 렌더링을 view에게 맡기는게 아니라 데이터를 객체로 만들고, 그 데이터는 JSON 형식으로 응답 바디로 보내진다.

코드의 메인 메서드는 Spring Boot의 SpringApplication.run() 메서드를 사용해 프로그램을 실행한다. 

### Spring Boot
Spring boot를 사용하면 빠르게 웹 어플리케이션을 만들수 있다. 이미 configure된 classpath 와 bean 을 저장한다. 저장할때 뭔가 빠졌으면 spring boot가 알아서 빠진걸 보충해준다. 인프라에 신경쓰지 않고 비즈니스 로직에 집중할 수 있다. 코드를 써주거나 파일을 써주는게 아니라, 어플리케이션을 시작할때 bean 과 setting 을 연결시켜서 어플리케이션 context 에 적용시킨다.

Spring boot 에서는 다른 서버의 API endpoint 를 호출할때 Rest Template을 많이 쓴다. CRUD Operation(get, post, delete, put)을 처리할때 쓰는 객체이다. 빈 설정을 먼저 해줘야 한다. build()를 사용해 만들어준다.
```
@Bean
public RestTemplate restTemplate(RestTemplate Builder builder){
    return builder.build();
}

@Bean
@Profile("!test")
public CommandLineRunner run(RestTamplate restTemplate) thorws Exception{
    return args -> {
        Quote quote = restTemplate.getForObject("http://localhost:8080/api/ramdom", Quote.class);
        log.info(quote.toString());
    }
}
```
예제에서 CommandLinerRunner 는 구동 시점에 실행되는 코드가 arg 값을 써야할때 쓰는 인터페이스다. 프로그램이 실행되면서 api/random 주소에서 JSON 응답을 가져온다. 가져온 응답은 Quote 객체에 매핑된다. 

Spring boot 에서 로그 사용시 두가지 방법이 있는데, LoggerFactory로 Logger를 사용하는 방법과 lombok를 쓸 경우 @Slf4j 어노테이션을 넣고 Logger를 사용할 수 있다.

먼저 LoggerFactory를 사용하는 경우에는Logger log = LoggerFactory.getLogger(class명.class) 이렇게 사용해주면 되고

lombok을 사용할 경우에는 훨씬 간단하게 클래스명 위에 @Slf4j 어노테이션을 입력한 후 log.메소드명(..); 이런 식으로 사용해주면 된다