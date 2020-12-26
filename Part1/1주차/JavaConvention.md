#Java Code Convention
###코딩 규칙(java convention)이 필요한 이유?
소프트웨어를 개발하는 일련의 모든 과정에 들어가는 비용 중 80퍼가 유지보수에 쓰임.
(직접 개발한 개발자가 그 소프트웨어의 유지보수를 담당하는 경우는 거의 없음)
따라서 코딩규칙을 지키며 소스코드 작성 시에 소프트웨어의 가독성이 높아져 더 빠른 시간안에 문제 해결이 가능해짐.

#비교적 생소한 자바 컨벤션 정리
1. 패키지 import 시에는 전체이름을 다 적는다.( * 사용하지 않는다.)
2. 패키지 이름은 소문자/ 클래스와 열거형을 선언할때는 파스칼 표기법 따른다.(ex : public class StudentScore) / 파스칼 표기법 : 모든 단어의 앞문자를 대문자로 표기)
3. 메서드, 메서드 매개변수, 지역 변수, 멤버 변수 이름은 카멜 표기법 따른다.
(카멜 표기법 : 첫단어의 앞문자 제외하고 단어의 앞문자를 대문자로 표기)
4. 메서드 이름은 동사로 시작한다.
Ex public int getAge() / setAge()
5. final 필드의 변수명, 열거형 멤버의 이름은 모두 대문자로 하며 밑줄로 각 단어를 분리  
Ex final int MAX_AGE = 1;  
public enum MyEnum{  
FUN,  
MY_AWESOME_VALUE  
};
6. 단순히 반복문에 사용되는 변수가 아닌 경우엔 i, e 같은 변수명 대신 index, employee처럼 하눈에 알아볼 수 있는 변수명을 사용
7. public 멤버 변수 대신 getter와 setter 메서드를 사용한다.
8. double이 반드시 필요한 경우가 아닌 이상 부동 소수점 값에 f를 붙여준다.
9. switch문에 항상 default: 케이스를 넣는다.
10. switch case문 끝에 break를 넣지 않고 그 바로 아래 case문의 코드를 실행하고 싶은 경우“// intentional fallthrough“ 라는 주석을 추가한다.
11. switch 문에서 default: 케이스가 절대 실행될 일이 없는 경우, default: 안에 assert (false) 란 코드를 추가하거나 예외를 던진다.
○ **assert 사용법**(assert :    
1) assert experssion1;  
인자로 boolean으로 평가되는 표현식 또는 값을 받아서 참이면 지나가고, 거짓이면 AssertionError예외 발생  
2) assert expression1: expression2;  
Expression1이 거짓인 경우 expression2 값이 예외 메시지로 보여진다.

참고사이트 : https://offbyone.tistory.com/294

12.  재귀 메서드는 이름 뒤에 Recursive를 붙인다.
public void fibonacciRecursive();

13. 클래스 안에서 멤버변수와 메서드의 등장 순서는 다음을 따른다.  
a.	public 멤버변수  
b.	default 멤버변수	//같은 패키지 내에서만 접근 가능한 멤버변수  
c.	protected 멤버변수  
d.	private 멤버변수  
e.	생성자  
f.	public 메서드  
g.	default 메서드	// 인터페이스 내부에서 함수 몸체가 있는 메서드(인터페이스 구현한 클래스에서 재정의 가능)  
h.	protected 메서드  
i.	private 메서드  

14. 클래스는 각각 독립된 소스파일에 있어야한다. 단, 작은 클래스 몇 개를 같이 넣어두는 것이 상식적일 경우 예외를 허용한다.
15. 변수 가리기(variable shadowing)은 허용하지 않는다. (variable shadowing : 인스턴스 변수와 지역 변수의 이름이 동일한 경우 메서드 안에서 변수를 인쇄(액세스) 할 때 지역 변수 값이 인쇄된다.
16.  var 키워드를 사용하지 않으려 노력한다.(단, 대입문의 우항에 자료형이 명확하게 드러나는 경우 또는 데이터형이 중요하지 않은 경우는 예외를 허용한다. 또, Iterable/ Collection에 var을 사용하거나 우항의 new 키워드를 통해 어떤 개체가 생성되는지 알 수 있는 등이 허용되는 경우의 좋은 예이다.)  
Ex) var employee = new Employee();
17. 메서드의 매개변수로 null을 허용하지 않는 것을 추구한다. 사용할 경우, 변수명 뒤에 OrNull을 붙인다. 
18. 메서드에서 null을 반환하지 않는 것을 추구한다. 하지만 때론 예외를 던지는 것을 방지하기 위해 그래야 할 경우도 있다.
19. 메서드를 오버라이딩 할 땐 언제나 @Override 어노테이션을 붙인다.
20. IntelliJ를 사용할 경우 탭을 사용하지만 다른 IDE를 사용할 경우는 실제 탭 문자 대신 띄어쓰기 4칸을 사용한다.
21. if문은 중괄호 안에 한 줄만 있더라도 반드시 중괄호 사용한다.
22. if문은 다음의 중괄호 스타일 사용  
if( ) { //코드 생략
} else if {
} else {
}

23. 특정 조건이 반드시 충족되어야한다고 가정하고 짠 코드모든 곳에 assert를 사용한다. Assert는 복구 불가능한 조건이다.
24. 외부로부터 들어오는 데이터의 유효성은 외부/내부 경계가 바뀌는 곳에서 검증하고 문제가 있을 경우 내부 메서드로 전달하기 전에 반환해 버린다. 이는 경계를 넘어 내부로 들어온 모든 데이터는 유효하다고 가정한다는 뜻이다. 따라서, 내부 메서드에서 예외(exception)을 던지지 않으려 노력한다. 예외는 경계에서만 처리하는 것을 원칙으로 한다. (설명 워드에)


####한줄 띄우기(Blank Lines)
+ 메서드들 사이에서
+ 메서드 안에서의 지역변수와 그 메서드의 첫번째 문장 사이
+ 블록 주석(/*~*/형태의 여러 줄 주석) 또는 한줄 주석 이전에
+ 가독성을 향상시키기 위한 메서드 내부의 논리적인 섹션들 사이에

-다음의 경우는 두 줄 띄우기  

+ 소스파일의 섹션들 사이에서
+ 클래스와 인터페이스의 정의 사이에서

####공백
+ 괄호와 함께 나타나는 키워드는 공백으로 나누어야한다.
+ .을 제외한 모든 이항 연산자는 연산수들과는 공백으로 분리되어져야한다. 단, 단항 연산자(++ 혹은 --)의 경우에는 사용해선 안된다.
+ for문에서 사용하는 세 개의 식들은 공백으로 구분  
for (expr1; expr2; expr3)
+ 변수의 타입을 변환하는 캐스트(cast)의 경우에는 공백으로 구분해야 한다.  
myMethod((byte) aNum, (Object) x);  
myMethod((int) (cp + 5), ((int) (i + 3)) + 1);
  
####시작 주석
/*  
* 클래스 이름  
*  
* 버전 정보  
*  
* 날짜  
*    
* 저작권 주의  
*/


참고  
http://kwangshin.pe.kr/blog/java-code-conventions-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%94%A9-%EA%B7%9C%EC%B9%99/	(밑 페이지 번역본)
https://www.oracle.com/java/technologies/javase/codeconventions-contents.html
