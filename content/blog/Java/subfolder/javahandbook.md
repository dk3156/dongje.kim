---
title: "Java Handbook"
date: 2023-11-27
mainSectionTitle: "hugo"
---
문자와 숫자간의 변환
숫자를 문자로
숫자 + '0' -> 문자
문자를 숫자로
문자 - '0' -> 숫자

문자열로의 변환
숫자를 문자열로
숫자 + "" -> 문자열
문자를 문자열로
문자 + "" -> 문자열
```
class Variable4_2 {
    public static void main(String[] args) {
        String s1 = "A" + "B"; // "AB"
        System.out.println("s1 = " + s1);

        // 문자열은 문자열하고만 결합할 수 있기 때문에
        // 숫자를 문자열로 바꾼 다음에 결합 합니다.
        String s2 = "" + 7;
        // "" + 7 => "" + "7" = "7"
        System.out.println("s2 = " + s2);

        // 문자열 결합 순서에 의한 차이 확인!
        String s3 = "" + 7 + 7;
        // "" + 7 + 7 => "7" + 7 => "7" + "7" = "77"
        System.out.println("s3 = " + s3);

        String s4 = 7 + 7 + "";
        // 7 + 7 + "" => 14 + "" = "14"
        System.out.println("s4 = " + s4);
    }
}
```
문자열을 숫자, 문자로 변환

문자열을 숫자로

Integer.parseInt("문자열")

Double.parseDouble("문자열")

문자열을 문자로

"문자열".charAt(0)


참조형
기본형을 제외한 나머지 타입을 뜻합니다.
String, System
참조형 변수는 null 또는 메모리 주소를 저장합니다.
null 은 '어떤 객체의 주소도 저장되지 않음' 을 뜻합니다.
타입에 관계없이 변수의 크기가 항상 4byte 입니다. (JVM이 64bit일 경우 8byte)
4byte는 2진수로 대략 40억개로, 40억byte(4GB)의 메모리를 다룰 수 있습니다.
```
class Variable3_5 {
    public static void main(String[] args) {
        // Date import 필요!
        Date date; // 참조형 변수 date 를 선언
        date = new Date(); // date 에 객체의 주소를 저장 , new 는 객체를 생성하는 명령어

        System.out.println(date); // Wed Jan 11 20:54:45 KST 2023
    }
}
```