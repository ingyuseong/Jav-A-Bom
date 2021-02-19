# 제네릭(Generic)
어떤 값이 하나의 참조 자료형이 아닌 여러 참조 자료형을 사용할 수 있도록 프로그래밍 하는 것을 제네릭(Generic) 프로그래밍이라고 한다. 참조 자료형이 변환될 때 이에 대한 검증을 컴파일러가 하므로 안정적이다. '컬렉션 프레임워크'도 많은 부분이 제네릭으로 구현되어 있다.
### 제네릭의 필요성 
+ 어떤 변수가 여러 참조 자료형을 사용할 수 있도록 Object클래스를 사용하면 다시 원래 자료형으로 반환해주기 위해 매번 형변환을 해야 하는 번거로움이 있다.
+ 따라서 여러 참조 자료형이 쓰일 수 있는 곳에 특정한 자료형을 지정하지 않고, 클래스나 메서드를 정의한 후 사용하는 시점에 어떤 자료형을 사용할 것인지 지정한다.   

		public class SimpleArrayList {	/*SimpleArrayList.java*/
    		private int size;
    		private Object[] elementData = new Object[5];
	
    		public void add(Object value) {
        		elementData[size++] = value;
    		}
	
    		public Object get(int idx) {
        		return elementData[idx];
    		}
		}

		public class SimpleArrayListTest {	/*SimpleArrayListTest.java*/
    		public static void main(String[] args) {
        		SimpleArrayList list = new SimpleArrayList();

        		list.add(50);
        		list.add(100);

        		Integer value1 = (Integer) list.get(0);
        		Integer value2 = (Integer) list.get(1);

        		System.out.println(value1 + value2);
    		}
		}

		
+ 위의 코드를 보면, add()메서드는 파라미터로 Object를 받기어 어떤 데이터 타입도 모두 받을 수 있다. 고로 list.get()부분에서 형변환만 잘 시켜주면 어떤 데이터 타입이든 저장할 수 있다. 만약 add(50)에 들어갈 50이 Integer이 아닌 String이 된다면?

		 public class SimpleArrayListTest {	/*SimpleArrayListTest.java*/
    		public static void main(String[] args) {
       			SimpleArrayList list = new SimpleArrayList();
	
        		list.add("50");  // 달라진부분
        		list.add("100"); // 달라진부분
	
        		Integer value1 = (Integer) list.get(0);
        		Integer value2 = (Integer) list.get(1);
	
        		System.out.println(value1 + value2);
    		}
		}

+ add() 메서드는 Object 타입은 모두 받을 수 있으므로 String, Integer 모두 인자로 줄 수 있다. get() 메서드도 Object 타입을 반환하기 때문에 `Integer value1 = (Integer) list.get(0);` 이라는 코드에는 문법적으로 문제가 없다. 
+ 실제로 컴파일이 잘 되지만, 실행하면 런타임에 오류가 발생한다. 
> 컴파일 타임 에러는 컴파일 시 발생하는 에러, 문법 상의 에러 등을 말한다.  
>> ex) ';'이 누락된 문법 에러(Syntax 오류), 괄호가안 맞는 등 구문 에러, classpath에 누락 된 클래스 등  
>  
> 런타임 에러는 실행 시 발생하는 에러, 프로그램이 컴파일 된 후 실행하면서 발생하는 에러, 설계 미숙으로 발생하는 에러가 있다.  
>> ex) NullPointerException, 무한루프, 0으로 나누는 경우 등

		Exception in thread "main" 	java.lang.ClassCastException:
    		java.lang.String cannot be cast to java.lang.Integer
    		at com.example.java.generics.basic.SimpleArrayListTest.main(SimpleArrayListTest.java:11)
+ 잘못된 타입 캐스팅이 이루어졌다는 오류메시지이다. String을 넣어놓고서 Integer로 형 변환 했기때문.

		public class GenericArrayList<T> {	/*GenericArrayList.java*/

    		private Object[] elementData = new Object[5];
    		private int size;

    		public void add(T value) {
        		elementData[size++] = value;
    		}

    		public T get(int idx) {
        		return (T) elementData[idx];
    		}
		}

+ 위의 코드처럼, <T>로 표현한 것이 제네릭이다. GenericArrayList 클래스는 객체를 생성할 때 타입을 지정하면, 생성되는 객체 안에서는 T의 위치에 지정한 타입이 대체되어서 들어가는 것처럼 컴파일러가 인식한다. 다시 말해, Raw타입으로 사용하는데 컴파일러에 의해 필요한 곳에 형변환 코드가 추가된다.
>>Raw type은 타입 파라미터가 없는 제네릭 타입을 의미한다. `GenericArrayList g = new GenericArrayList();`라는 코드에서 g가 Raw Type  
>> 타입 파라미터 = 제네릭 타입 : <>사이에 들어가는 매개변수

		class Test {	/*Test.java*/
    		public static void main(String[] args) {
       		 GenericArrayList<Integer> intList = new GenericArrayList<>();
      		  intList.add(1);
    	   	  intList.add(2);

      		  int intValue1 = intList.get(0); // 형변환이 필요없다
      		  int intValue2 = intList.get(1); // 형변환이 필요없다

     		   // String strValue = intList.get(0); // 컴파일에러
			}
		}
+ 제네릭은 형 변환이 필요없다는 것, 지정한 타입과 다른 타입의 참조변수를 선언하면 컴파일타임에 오류가 발생한다는 점이 특징이다.


## 제네릭 타입(class< T >, interface< T >)
제네릭 타입은 타입을 파라미터로 가지는 클래스와 인터페이스를 말한다. 제네릭 타입은 클래스 또는 인터페이스 이름 뒤에 "<>"가 붙고 사이에 타입 파라미터가 위치한다. 실제 코드에서 사용하기 위해선 타입 파라미터에 구체적인 타입을 지정해줘야 한다. 타입 파라미터를 사용해야 하는 이유는?
> 위의 코드에서 알 수 있듯이, 타입 파라미터를 사용하지 않는다면 항상 형 변환 해줘야 한다는 번거로움이 있다. 또한, 형 변환이 빈번해지면 전체 프로그램 성능에 좋지 못한 결과를 가져올 수 있다. 

고로, 타입 파라미터 T를 사용해 Object 타입을 모두 T로 대체한다. T는 객체를 생성할 때 구체적인 타입으로 변경된다.
예를 들어, ```GenericArrayList<String> strList = new GenericArrayList<String>();```로 객체 생성 시에 제네릭 클래스는 다음과 같이 자동으로 재구성된다.

	public class GenericArrayList<String> {	/*GenericArrayList.java*/

    	private Object[] elementData = new Object[5];
    	private int size;

    	public void add(String value) {
        	elementData[size++] = value;
    	}

    	public String get(int idx) {
        	return (String) elementData[idx];
    	}
	}

이와 같이 제네릭은 클래스를 설계할 때 구체적인 타입을 명시하지 않고, 타입 파라미터로 대체했다가 실제 클래스가 사용될 때 구체적인 타입을 지정함으로써 타입 변환을 최소화시킨다.

## 멀티 타입 파라미터(class<K,V,...>, interface<K,V,....>)
제네릭 타입은 두 개 이상의 멀티 타입 파라미터를 사용할 수 있다. 각 타입 파라미터를 콤마로 구분한다. 

	public class Product<T, M> {
		private T kind;
		private M model;
	
		public T getKind() { return this.kind; }
		public M getModel() { return this.model; }
	
		public void setKind(T kind) { this.kind = kind; }
		public void setModel(M model) { this.model = model; }
	}

ProductExample 클래스에서 Product<Tv, String> 객체와 Product<Car, String>객체를 생성한다. (Tv, Car클래스 존재한다고 가정)

	public class ProductExample {
		public static void main(String[] args) {
			Product<Tv, String> product1 = new Product<Tv, String>();
			product1.setKind(new Tv());
			product1.setModel("스마트Tv");
			Tv tv = product1.getKind();
			String model = product1.getModel();
		
			Product<Car, String> product2 = new Product<Car, String>();
			product2.setKind(new Car());
			product2.setModel("소나타");
			Car car = product2.getKind();
			String carModel = product2.getModel();
		}
	}

제네릭 타입 변수 선언과 객체 생성을 동시에 할 때 타입파라미터 자리에 구체적인 타입을 지정하는 코드가 중복해서 나와 다소 복잡해질 수 있다. 자바7부터 제네릭 타입 파라미터의 중복기술을 줄이기 위해 다이아몬드 연산자 <>를 제공한다. 자바 컴파일러는 타입 파라미터 부분에 <>연산자 사용하면 타입 파라미터를 유추해서 자동으로 설정해준다.
`Product<Tv, String> product1 = new Product<>();` 이런 식으로 <>연산자 사용해 <>안 생략가능.

## 제네릭 메서드(<T, R> R method(T t))
제네릭 메서드는 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메서드를 일컫는다. 제네릭 메서드 선언 방법은 다음과 같다.

	public <타입 파라미터,...> 리턴 타입 메서드명(매개변수,...){...}
	
제네릭 메서드는 두 가지 방식으로 호출할 수 있다.

	리턴 타입 변수 = <구체적 타입> 메서드명(매개값);		//명시적으로 구체적인 타입을 지정
	리턴 타입 변수 = 메서드명(매개값);					//매개값을 보고 컴파일러가 구체적 타입을 추정

정적 제네릭 메서드 boxing()을 정의하고 BoxingMethodExample클래스에서 호출해보자.

	public class Box<T> {		//Box.java
		private T t;
		public T get() { return t; }
		public void set(T t) { this.t = t; }
	}

	public class Util {			//Util.java
		public static <T> Box<T> boxing(T t){
			Box<T> box = new Box<T>();
			box.set(t);
			return box;
		}
	}

	public class BoxingMethodExample {		//BoxingMethodExample.java
		public static void main(String[] args) {
			Box<Integer> box1 = Util.<Integer>boxing(100);	//타입 파라미터를 명시적으로 Integer로 지정
			int intValue = box1.get();
		
			Box<String> box2 = Util.boxing("홍길동");			//매개값 보고 타입 파라미터 추정
			String strValue = box2.get();
		}
	}

## 제한된 타입 파라미터(T extends 상위타입) : Bounded Type Parameter
타입 파라미터에 지정되는 구체적인 타입을 제한할 필요가 있다. 상위 타입은 클래스뿐만 아니라 인터페이스도 가능하다. 인터페이스라고 해서 `implements`를 사용하진 않는다. 타입 파라미터에 지정되는 구체적인 타입은 상위 타입이거나 상위 타입의 하위 또는 구현 클래스만 가능하다.   
주의할 점은 메서드의 중괄호{}안에서 타입 파라미터 변수로 사용 가능한 것은 상위 타입의 멤버(필드, 메서드)로 제한된다. 하위 타입에만 있는 필드와 메서드는 사용할 수 없다.

	public class Util {															/*Util.java*/
		public static <T extends Number> int compare(T t1, T t2){
			double v1 = t1.doubleValue();	//Number의 doubleValue() 사용
			double v2 = t2.doubleValue();	//Number의 doubleValue() 사용
			return Double.compare(v1, v2);
		}
	}

	public class BoundedTypeParameterExample {							/*BoundedTypeParameterExample.java */
		public static void main(String[] args) {
			//String str = Util.compare("a", "b"); (x) -> String은 Number 타입이 아님.
			int result1 = Util.compare(10, 20);	//int -> Integer으로 자동 Boxing
			System.out.println(result1);		//-1
		
			int result2 = Util.compare(4.5, 3);	//double -> Double 자동 Boxing
			System.out.println(result2);		//1
		}
	}
[Number 클래스](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Number.html#doubleValue())  
숫자 값을 나타내는 플랫폼 클래스(AtomicInteger, AtomicLong, BigDecimal, BigInteger, Byte, Double, DoubleAccumulator, DoubleAdder, Float, Integer, Long, LongAccumulator, LongAdder,Short)의 최상위 클래스.

## 와일드카드 타입 <?>, <? extends ...>, <? super ...>
코드에서 ?를 일반적으로 와일드카드(wildcard)라고 부른다. 제네릭 타입을 매개값이나 리턴 타입으로 사용할 때 구체적인 타입 대신에 와일드 카드를 다음과 같이 세 가지 형태로 사용할 수 있다.

+ 제네릭 타입<?> : Unbounded Wildcards(제한없음)
	+ 타입 파라미터를 대치하는구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있다.
+ 제네릭 타입<? extends 상위타입> : Upper Bounded Wildcards(상위 클래스 제한)
	+ 타입 파라미터를 대치하는 구체적인 타입으로 상위 타입이나 상위 타입의 하위 타입만 올 수 있다.
+ 제네릭 타입<? super 하위타입> : Lower Bounded Wildcards(하위 클래스 제한)
	+ 타입 파라미터를 대치하는 구체적인 타입으로 하위 타입이나 하위 타입의 상위 타입만 올 수 있다.  

			public class Course<T> {
				private String name;
				private T[] students;
	
				public Course(String name, int capacity) {
					this.name = name;
					students = (T[]) (new Object[capacity]);
				}
	
				public String getName() { return name; }
				public T[] getStudents() { return students; }
				public void add(T t) {
					for(int i = 0; i < students.length; i++) {
						if(students[i] == null) {
							students[i] = t;
							break;
						}
					}
				}
			}

![클래스구조](https://t1.daumcdn.net/cfile/tistory/999C1A505B1B850828)

위와 같은 클래스 구조가 있다.   

+ **Course<?>**   
수강생은 모든 타입<u>(Person, Worker, Student, HighStudent)</u>이 될 수 있다.
+ **Course<? extends Students>**  
수강생은 <u>Student와 HighStudents</u>만 될 수 있다.
+ **Course<? super Worker>**  
수강생은 <u>Worker와 Person</u>만 될 수 있다.	 

### 제네릭을 사용할 수 없는 경우
제네릭으로 배열 생성불가(선언은 가능! `private T[] students;`)
>이유는 new 연산자 때문이다. new 연산자는 heap 영역에 충분한 공간이 있는지 확인한 후 메모리를 확보하는 역할을 한다. 충분한 공간이 있는지 확인하려면 타입을 알아야하는데 컴파일 시점에 타입 T가 뭔지 모른다면 무엇인지 알 수 없어서 `new T[5]`와 같이 제네릭으로 배열을 생성할 수 없다.
> 하지만, 생성이 아예 불가능한 건 아니다. `new T[n]` 형태로는 배열을 생성할 수 없지만, `(T[]) (new Object[n])` 으로는 생성이 가능하다.

static 변수에는 제네릭 사용불가
> static 변수는 인스턴스에 종속되지 않는 클래스 변수로써 모든 인스턴스가 공통된 저장공간을 공유하게 되는 변수다.
> static 변수는 제네릭을 사용하게되면 여러인스턴스에서 어떤 타입으로 공유되어야 할지 지정할 수가 없어서 사용할 수 없다. static 변수는 값 자체가 공유되기 때문이다. 값 자체가 공유 되려면 타입에 대한 정보도 있어야 한다.


static 메서드에는 제네릭 사용가능
>하지만 static 메서드의 경우 메서드의 틀만 공유된다고 생각하면 된다. 그리고 그 틀 안에서 지역변수처럼 타입 파라미터가 다양하게 오가는 형태로 사용될 수 있다. 이는 인스턴스 메서드의 경우에도 마찬가지다. 클래스에 정의된 타입 파라미터와는 전혀 별개로 제네릭 메서드는 자신만의 타입 파라미터를 가진다.




참고  


+ 이것이 자바다  
+ https://www.tutorialspoint.com/java/java_generics.htm?utm_source=7_&utm_medium=affiliate&utm_content=5f34cd37cdf1050001b09537&utm_campaign=Admitad&utm_term=7152ee40dd689b81911af75e73bce735
+ https://yaboong.github.io/java/2019/01/19/java-generics-1/
+ https://spaghetti-code.tistory.com/35
+ http://happinessoncode.com/2018/02/08/java-generic-raw-type/
+ https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Number.html#doubleValue()