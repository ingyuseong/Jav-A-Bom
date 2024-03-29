# Jav-A-Bom

![](https://media.vlpt.us/images/harang/post/4933a739-0597-4b93-89f6-17d8987e38c4/%EC%9E%A1%EC%95%84%EB%B4%84%20%EC%8D%B8%EB%84%A4%EC%9D%BC.png)

## 학습 목표
1. `Clean Code`, `Test Code`, `OOP`, `Refactoring` 연습.
2. [이펙티브 자바](https://g.co/kgs/ja3V4R)를 메인으로 `Java` 학습.
3. [토비의 스프링 3.1](http://www.yes24.com/Product/Goods/7516911)을 메인으로 `Spring` 학습.
4. `Java`와 `Spring`을 이용한 실전 프로젝트. 단순히 구현에 그치지 않는다. 1~3에서 배운 모든 개념을 녹이고, 깊이 있는 기술적 시도해보기.


## 학습 방법
`Java` 개발자라 몰입하고 코드 작성하기. `Clean Code`, `Test Code`에 대해 끊임없이 집착하기.

### Part1. Java와 객체지향(OOAD+OOP)

[개체지향 프로그래밍 및 설계(Java)](https://www.udemy.com/course/object-oriented-programming-and-design-by-pocu/) 강의 스터디를 메인으로 진행된다. 강조하지만, 강의의 모든 부분을 대충 넘어가지 않고, **조금이라도 이해가 되지않는다면 Issue에 추가하고 이해될 때까지** 학습한다.


#### 스터디 사이클
스터디 사이클은 한 주 단위로 구성된다. 매주 스터디 리더 1명을 정하여 진행한다. 스터디 리더는 정해진 학습 범위를 예습하고, 범위 내의 가장 중요한 개념들을 선별해 키워드로 정리하고, 이를 Issue로 올린다. 모든 스터디원은 최소 하나 이상의 키워드를 맡아 심화적으로 학습하고 발표한다. 발표 or 공유 자료 중 개인이 작성한 코드가 있다면, 스터디원들과 코드를 리뷰한다.

* 개별학습(일~월)
  1. 정해진 한 주만큼의 범위를 **탐구**(강의 내 실습 / 서칭 / 직접 코딩).
  2. 학습 내용 중 모르는 부분 & 공유하고자 하는 개별 학습 내용은 [잡아봄 Repo](https://github.com/staycozyboy/Jav-A-Bom)의 Issue에 Comment로 추가한다.
  3. 지난주 미해결 탐구 과제가 있다면 이에 대해 학습.
  4. 모든 멤버는 토요일 스터디 시작 전까지 본인이 맡은 키워드에 대해 탐구하고, 이를 md파일로 정리하여 잡아봄 Repo에 update한다.
  5. 스터디 리더는 추가적으로 Issue의 질문 및 공유사항들을 총 정리하여 발표 준비.

* 발표 및 토론 스터디(토)
  1. 각 스터디원의 발표 + 질의응답.
  2. Topic과 Issue를 중심으로 서로 문답하며 토론 진행.
  3. 특히 스터디 리더는 책임지고 모든 질문에 대답할 수 있도록 깊게 공부한다.
  4. 모두 모르는 내용에 대해서는 미해결 Issue로 정하여, 스터디 이후 각자 탐구한다. 이는 다음 회차 스터디에서 다룬다.
  
* 회고(토, 2주~4주에 한 번)
  1. 주기적으로 한 번씩 각자 학습했던 내용, 스터디하며 느낀 점 등등 자유롭게 각자의 2주를 돌아보고 글로 정리한다.
  2. 토요일 스터디가 끝내고 각자의 회고를 공유하며 이야기 나눈다.

#### `OOP` 프로그램 과제
스터디에 앞서(&진행 하며), `Java` Basic, `Clean Code`, `Test Code`, `TDD`, `OOP`, `DDD`, `Refactoring`의 개념을 개별적으로 학습한다. (필요시 중요한 개념을 주제를 선정하여 발표 및 토론하며 학습한다.) 아래의 소스를 참고.

* [Java Code Convention](https://google.github.io/styleguide/javaguide.html)
* [우아한 테크세미나: 우아한 객체지향](https://www.youtube.com/watch?v=dJ5C4qRqAgA&t=1637s)
* [소트웍스 앤솔러지](http://www.yes24.com/Product/Goods/3290339)
  * [객체지향 생활체조](https://developerfarm.wordpress.com/2012/02/03/object_calisthenics_summary/)
* [Udacity commit message style guide](https://udacity.github.io/git-styleguide/)

필요시 약간의 로직을 포함하면서 요구사항이 명확한 프로그램을 구현하는 과제를 수행한다. 이때, 사전에 약속한 제약사항을 엄격히 준수하며 프로그래밍한다. Part 1의 모든 학습은 기본적으로 자바지기 박재성님의 [의식적인 연습으로 TDD, 리팩토링 연습하기](https://www.youtube.com/watch?v=cVxqrGHxutU)를 기준으로 삼아 진행한다. 필요시 회의를 통해 유동적으로 룰을 추가/변경할 수 있다([예시 참고](https://github.com/kimhodol/java-racingcar)). 이후 스터디에서 각자의 코드를 설명하고 스터디원들에게 리뷰받는다. 이 과제의 핵심 가치는 **스터디원들의 코드 리뷰다.** 열심히 리뷰해주자.

#### 타임라인
* 12.19 : Java & OO 스터디(Part1) 시작
* 12.19 ~ 12.25 : *과목 소개 + Java 언어의 기본 문법 + 개체지향 프로그래밍의 필요성* 파트 학습
* 12.26 : 스터디 & Part1 OT + 1주차 스터디
* 12.26 ~ 21.01.01 : *클래스 + 개체 모델링 1* 파트 학습
* 21.01.02 : 2주차 스터디
* 01.02 ~ 01.08 : *static, 싱글턴, 내포 클래스 + 상속* 파트 학습
* 01.09 : 3주차 스터디 + 간단히 스터디 회고
* 01.10 ~ 01.15 : *상속을 이용한 개체 모델링 + 상속 vs 컴포지션* 파트 학습
* 01.16 : 4주차 스터디
* 01.17 ~ 01.22 : *다형성 + 추상 메서드/클래스 + 인터페이스 + 인터페이스 vs 구현* 파트 학습
* 01.23 : 5주차 스터디
* 01.24 ~ 01.29 : *디자인 패턴 + 예외* 파트 학습
* 01.30 : 6주차 스터디
* 01.31 ~ 02.05 *SOLID 설계 정신 + 소수설에서 태어난 다양한 주장들 + 강의를 마무리 하며* 파트 학습
* 02.06 7주차 스터디 및 **Part 1 회고** / Part 2 OT

## Reference
* [자바봄 스터디](https://javabom.tistory.com/)
* [WeareSoft](https://github.com/WeareSoft)
* [우아한 테크코스](https://woowacourse.github.io/)
* 이미지 출처 : https://codeprof.net/java.html
