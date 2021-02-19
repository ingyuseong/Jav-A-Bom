# 생성자(Constructor)  
생성자는 new 연산자와 같이 사용되어 클래스로부터 객체를 생성할 때 호출되어 객체의 초기화를 담당한다. 객체 초기화란 필드를 초기화하거나, 메서드를 호출해서 객체를 사용할 준비를 하는 것을 말한다. new 연산자에 의해 생성자가 성공적으로 실행되면 힙(heap) 메모리에 객체가 생성되고 <u>객체의 주소가 리턴된다.</u>  

+ 자바의 기본 자료형을 제외한 모든 자료형은 포인터형(포인터 : 메모리 주소를 저장하는 데이터)이나 다름없다. 다만, C에서 가능한 포인터 연산이 불가능 할 뿐이다. C에서는 개체(구조체)를 포인터 혹은 값으로 둘 다 전달 가능하지만, Java는 무조건 포인터로만 전달 가능하다(참조형). cf)기본형과 Wrapper클래스는 값만 전달하는것도 가능하다.   

		int a = 10;
		int b = a;
		b = 20;
		System.out.println(a);		//10
		System.out.println(b);		//20
		
		Integer i1 = new Integer(5);
		Integer i2 = new Integer(50);
		
		i2 = i1;
		i2 = 40;
		
		System.out.println(i1);		//5		값만 전달됐음을 알 수 있다.
		System.out.println(i2);		//40

그리고 프로그래머가 생성자를 명시하지 않았을 경우에는, java compiler(JVM 내부)에서 자동으로 디폴트 생성자를 생성해준다. 하지만, 클래스에 명시적으로 선언한 생성자가 한 개라도 있으면, 컴파일러는 기본 생성자를 추가하지 않는다. 생성자를 명시적으로 선언하는 방법은 다음과 같다.

	클래스(매개변수 선언, ...) {
		//멤버변수 초기화 코드  
	}
   
멤버변수 초기화코드(생성자 내부)에서 외부에서 받아오는 매개변수랑 객체의 멤버변수를 구분하는 방법은 `this.필드`로 객체 자신을 참조하는 명령어 this를 사용해 구분한다. 

## 생성자 오버로딩
외부에서 제공되는 다양한 데이터들을 이용해서 객체를 초기화하려면 생성자도 다양화될 필요가 있다. 예를 들어 외부에서 제공되는 데이터가 없다면 기본 생성자로 객체를 생성해야하고, 외부에서 1개의 데이터가 제공되면 1개의 데이터를 기반으로 멤버변수를 초기화해 객체를 생성하고 2개의 데이터가 제공되면 2개의 데이터를 초기화해 객체를 생성한다. 이렇듯 다양한 방법으로 객체를 생성할 수 있도록 생성자 오버로딩을 한다. (생성자 오버로딩 : 매개 변수를 달리하는 생성자를 여러개 선언하는 것)   

	public Car() {}
	public Car(String model) {}
	public Car(String model, String color) {}
	public Car(String model, String color, int maxSpeed) {}

주의할 점은 매개 변수의 타입과 개수 그리고 선언된 순서가 똑같을 경우 매개변수 이름만 바꾸는 것은 생성자 오버로딩이라고 볼 수 없다.

	public Car(String color, String model) {}
	public Car(String model, String color) {}

생성자 오버로딩이 많아질 경우 생성자 간의 중복된 코드가 발생할 수 있다. 중복된 코드는 협업 시 다른 프로그래머로부터 실수를 유발한다. 따라서 매개변수가 적은 생성자에서 매개변수가 많은 생성자를 호출하는 방식으로 중복된 코드를 피할 수 있다. 

	public Car(String model) {
		this(model, "은색", 250);
	}
	public Car(String model, String color) {
		this(model, color, 250);
	}
	public Car(String model, String color, int maxSpeed) {
		this.model = model;
		this.color = color;
		this.maxSpeed = maxSpeed;
	}
개체 생성후에 값을 대입한다.  
	
	Car car1 = new Car();
	
	car1.model = "Sonata";
	car1.color = "은색";
	car1.maxSpeed = 250;
그리고 포큐 수업에서 알 수 있듯이 개체 생성 후에 값을 대입하면 좋지 않다. 거기엔 3가지 이유가 있다.    

+  개념상의 문제(ex. 공장에서 찍어낸 물건이 속이 비었다?)  
+  후 조건의 문제  
	1. 선조건 : 메서드가 호출 될 때 참이어야하는 것. 
	2. 후조건 : 메서드가 성공적으로 완료된 후 참이어야하는 것.   
생성자의 후조건은 '개체의 상태는 개체의 생성과 동시에 유효하다'이다. 
+  사용자를 고려하지 않은 문제  
사용자의 실수를 유발한다. (사용자: 내가 만든 클래스를 사용하는 프로그래머 혹은 나 자신)
	+ 클래스 내부의 어떤 멤버 변수를 초기화해야하는지 모른다.
	+ 나중에 멤버변수가 추가되는 것에 대응하기 어렵다.(새로운 멤버변수가 추가되고 초기화해주지 않아도 컴파일 오류 발생 X)
	+ 어떤 값으로 초기화해줘야하는지 모른다.

따라서 개체 생성 직후에도 개체가 유효하기 위해 생성자 내부에서 멤버변수를 초기화한다.

## 생성자 참조(메서드 참조)
메서드 참조는 메서드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개 변수를 제거하는 것이 목적이다.  
메서드 참조에는 생성자 참조도 포함된다. 생성자를 참조한다는 것은 객체 생성을 의미한다. 단순히 메서드 호출로 구성된 람다식을 메서드 참조로 대치할 수 있듯이, 단순히 객체를 생성하고 리턴하도록 구성된 람다식은 생성자 참조로 대치할 수 있다.
### 람다식
람다식은 익명 함수(anonymous fuction)을 생성하기 위한 식으로 객체 지향 언어보다는 함수 지향 언어에 가깝다. 자바에서 람다식을 수용한 이유는 자바 코드가 매우 간결해지고, 컬렉션의 요소를 필터링하거나 매핑해서 원하는 결과를 쉽게 집계할 수 있기 때문이다. 람다식의 형태는 매개 변수를 가진 코드 블록이지만, 런타임 시에는 익명 구현 객체를 생성한다

다음 코드를 보면 람다식은 단순히 객체 생성 후 리턴만 한다.  

	(a, b) -> { return new 클래스(a, b)};

이런 경우 생성자 참조로 표현하면, 클래스 이름 뒤에 :: 기호를 붙이고 new 연산자를 기술하면 된다. 생성자가 오버로딩되어 여러 개가 있을 경우 컴파일러는 함수적 인터페이스의 추상 메서드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아 실행한다. 만약 해당 생성자가 존재하지 않으면 컴파일 오류가 발생한다.  

	클래스 :: new 
 
### 함수적 인터페이스 
함수적 인터페이스(functional interface)는 java.util.function 표준 API 패키지로 제공한다.(표준 API패키지 : java.lang/ java.util) 이 패키지에서 제공하는 함수적 인터페이스의 목적은 메서드 또는 생성자의 매개 타입으로 사용되어 람다식을 대입할 수 있도록 하기 위해서이다. 함수적 인터페이스는 크게 Consumer, Supplier, Function, Operator, Predicate로 나뉘는데 여기서는 Function에 대해서만 알아보겠다.
![](https://t1.daumcdn.net/cfile/tistory/217E3E3658B03FCC0F)

#### Function 함수적 인터페이스
Function 함수적 인터페이스의 특징은 매개 값과 리턴 값이 있는 applyXXX() 메서드를 가지고 있다. 이 메서드들은 매개 값을 리턴 값으로 매핑(타입 변환)하는 역할을 한다.
매개 변수 타입과 리턴 타입에 따라서 아래와 같은 Function 함수적 인터페이스들이 있다.

![](https://t1.daumcdn.net/cfile/tistory/241F824F55FF967109)


---
다시 넘어와서, 예제를 살펴보자. 다음 예제는 생성자 참조를 이용해서 두 가지 방법으로 Member 객체를 생성한다. 하나는 Function<String, Member> 함수적 인터페이스의 Member apply(String) 메서드를 이용해서 Member 객체를 생성하였고, 다른 하나는 BitFunction<String, String, Member> 함수적 인터페이스의 Member apply(String, String) 메서드를 이용해서 Member 객체를 생성하였다. 생성자 참조는 두 가지 방법 모두 동일하지만, 실행되는 Member 생성자가 다름을 알 수 있다.

	public class Member {
		private String name;
		private String id;
	
		public Member() {
			System.out.println("Member() 실행");
		}
	
		public Member(String id) {
			System.out.println("Member(String id) 실행");
			this.id = id;
		}
	
		public Member(String name, String id) {
			System.out.println("Member(String name, String id) 실행");
			this.name = name;
			this.id = id;
		}
	
		public String getId() {
			return this.id;
		}
	}
위와 같은 Member 클래스가 있다.

	import java.util.function.BiFunction;
	import java.util.function.Function;

	public class ConstructorReferencesExample {

		public static void main(String[] args) {
			Function<String, Member> function1 = Member :: new;			//생성자 참조
			Member member1 = function1.apply("angel");
		
			BiFunction<String, String, Member> function2 = Member :: new;		//생성자 참조
			Member member2 = function2.apply("신천사", "angel");
		}
	}

실행결과 

	Member(String id) 실행
	Member(String name, String id) 실행



  
참고 

+  이것이 자바다  
+  do it 자바 프로그래밍 입문  
+  https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html
+ https://eddyplusit.tistory.com/35
