# 자바 제네릭(Java Generics)의 이해
*시작에 앞서, 이 글은 책, 강의, 구글링등을 통해 개인적으로 학습 및 실습한 개념을 정리한 글이며, 정확하지 않은 내용이 있을 수 있습니다. 꾸준히 공부해가며 수정하겠습니다. 또한, 오류 지적은 언제나 환영입니다.*

이 글은 JCF, Lambda, Stream등 자바의 핵심 요소를 정확히 이해하기 위해 반드시 학습해야하는 제네릭(Generics)을 2편에 나누어 다루고 있다.

1. 제네릭의 이해
2. 제네릭의 활용; 컬렉션 프레임워크

## 개요
* 제네릭의 개념(What is Generics?)
* 제네릭을 사용하는 이유(Why use Generics?)
* 제네릭의 기본(Generics Basic Syntax)
* 와일드카드(Wildcard)

## 제네릭의 개념(What is Generics?)
* Generics add stability to your code by making more of your bugs detectable at compile time.(Java Documentation)
* 제네릭이 등장하면서 자료형에 의존적이지 않은 클래스를 정의할 수 있게 되었다.(윤성우의 열혈 Java 프로그래밍)

핵심은 위의 두 가지 사항이다. 간단하게 말하면, **Type을 파라미터화하여 컴파일시 구체적인 타입이 결정되도록 하는 것**이다. 이를 통해 Type과 관련된 프로그래머의 실수를 런타임이 아닌 컴파일 타임에 발견하고 코드의 안정성을 높일 수 있다. 이는 Java 5부터 도입되었다.

제네릭은 매우 다양하게 활용되고 있다. 이는 2편에서 다룰 컬렉션 프레임워크(JCF, Java Collection Framework)을 포함해 람다, 스트림등과 같은 자바의 핵심 요소와 깊은 관련이 있다. 따라서, 이러한 핵심 요소를 제대로 이해하기 위해선 제네릭에 대한 정확한 이해가 선행되어야 한다.

## 제네릭을 사용하는 이유(Why use Generics?)

#### 제네릭을 쓰지 않은 코드
먼저, **자료형에 의존적이지 않은 클래스**에 초점을 두고 다음의 예제를 살펴보자. 첫 번째 코드는 제네릭을 쓰지 않은 코드이다.

```java
class Box {
	private Object o;
    
    public void set(Object o) {
    	this.o = o;
    }
    
    public Object get() {
    	return o;
    }
}

class Example {
	public static void main(String[] args) {
    	Box intBox = new Box()
        
        intBox.set(new Integer(1));
        
        Integer intObject = (Integer)intBox.get(); // Casting
        
        System.out.println(intObject);
    }
}
```

문제없이 실행되는 코드이다. 이때, main 메서드에서 get 메서드를 호출하는 부분에 주목하자. **Box 인스턴스에서 내용물을 꺼널 때 캐스팅**을 해야함을 알 수 있다. 이는 그 자체로 번거로울 뿐만 아니라 더 심각한 문제를 야기할 가능성이 있다. main 메서드가 다음과 같이 작성될 경우를 살펴보자.

```java
class Example {
	public static void main(String[] args) {
    	Box intBox = new Box()
        
        intBox.set("1"); // Integer가 아닌 String을 인자로 넘겨줌.
        
        Integer intObject = (Integer)intBox.get(); // ClassCastException 발생
        
        System.out.println(intObject);
    }
}
```

위 코드는 실수로 Integer가 아닌 String 인스턴스를 인자로 넘겨준 코드다. Object로 인자를 받기 때문에 해당 라인은 넘어가지만, 다음 줄에서 Integer로 캐스팅하려는 과정에서 ClassCastException이 발생한다. 문제는 이러한 **오류가 컴파일 과정에서 발견되지 않고 런타임에 발견**되었다는 점이다. 컴파일 에러는 원인을 바로 찾을 수 있는 반면, 런타임 에러의 경우 원인을 쉽게 발견하기 힘들뿐만 아니라 심지어 명시적으로 실수가 드러나지 않을 수도 있다. 따라서, **모든 실수는 최대한 컴파일 단계에서 드러나는 것이 좋다.**

#### 제네릭을 사용하지 않았을 때의 문제점

위의 코드를 통해 살펴본 제네릭을 쓰지 않았을 때의 문제점은 다음과 같다.

1. 필요시 캐스팅을 해야 한다. -> 빈번하게 발생할 경우 성능 저하를 야기할 수 있다.
2. 자료형과 관련된 프로그래머의 실수가 컴파일 과정에서 드러나지 않는다.

#### 제네릭을 사용한 코드
다음은 제네릭을 사용한 코드이다. 이전 코드에 비해 직관적으로 확 와닿을 것이라 예상한다.

```java
class Box<T> {
	private T o;
    
    public void set(T o) {
    	this.o = o;
    }
    
    public T get() {
    	return o;
    }
}

class Example {
	public static void main(String[] args) {
    	Box<Integer> intBox = new Box<Integer>()
        
        intBox.set(new Integer(1));
        // intBox.set("1"); // Compile 과정 중 에러 발견!
        
        Integer intObject = intBox.get(); // X Casting
        
        System.out.println(intObject);
    }
}
```

먼저, Box의 인스턴스를 생성하는 라인을 보자. **인스턴스를 생성하는 과정에서 T를 Integer로 결정**하였다. 이 라인에 의해 컴파일 시 제네릭 타입이 Integer로 변환된다. 또한, get 메서드를 호출하는 라인을 확인하면 **캐스팅이 불필요해졌음**을 알 수 있다. 마지막으로, 이전과 같이 set 메서드의 인자를 String 인스턴스로 넘겨줄 경우, 제네릭에 의해 Integer or Integer를 상속하는 클래스만 인자로 넘겨받기 때문에, **컴파일 에러가 발생**하게 된다.

결국 제네릭을 사용함으로써 얻는 이점은 앞서 언급한 문제점들을 해결할 수 있다는 것이다. 다시한번 정리하자면, 제네릭을 사용하는 이유는 다음과 같다.

1. 불필요한 캐스팅을 하지 않아도 된다.
2. Type과 관련된 버그를 컴파일 타임에 발견할 수 있기 때문에, 코드의 안정성이 높아진다.

## 제네릭의 기본(Generics Basic Syntax)

### Terminology
* Type Parameter	`Box<T>` 에서 T
* Type Argument 	`Box<Integer>` 에서 Integer
* Parameterized Type(=제네릭 타입)	`Box<Integer>`

### 제네릭 클래스 Type Argument 제한
앞서 정의한 `Box<T>`의 Type Argument를 다음과 같이 제한할 수 있다.

```java
class Box<T extends Number> {
	private T o;
    
    public void set(T o) {
    	this.o = o;
    }
    
    public T get() {
    	// return o;
        return o.intValue();  // Number 클래스의 메서드 호출 가능!
    }
}
```

이는 제네릭 클래스의 **Type Argument를 Number 또는 이를 상속하는 하위 클래스로 제한**한 것이다. 이는 Type Argument가 Number 클래스의 메서드를 가지고 있음을 보장하기 때문에, **Type Argument를 제한할 경우 해당 클래스의 메서드를 호출할 수 있다.** 이는 인터페이스의 경우에도 마찬가지로 적용된다.

### 제네릭 메서드(+ Target Type)
클래스가 전부가 아닌 **일부 메서드에 대해서만 제네릭으로 정의하는 것도 가능**하며, **이렇게 정의된 메서드를 제네릭 메서드**라고 한다. 다음과 같이 정의할 수 있다.

`public static <T> Box<T> make(T o) {...}`

`Box<T>` 타입을 반환하고, `T` 타입을 매개변수로 넘겨받는 메서드이며, 맨 앞의 `<T>`는 `T`가 Type Parameter임을 명시하는 표시이다. 제네릭 클래스가 인스턴스 생성 시 Type이 결정되듯이, **제네릭 메서드는 메서드 호출시 Type이 결정**된다. 호출은 다음과 같이 이루어진다.

`Box<String> strBox = BoxFactory.make("Music");`

이때 컴파일러는 make 메서드에 전달되는 인자를 기준으로 `T`의 Type을 유추한다. 그럼 인자를 받지 않는 경우는 호출할 때 따로 Type을 표기해줘야할까? 다음의 예제를 보자.

`public static <T> Box<T> make() {...}  // 인자 받지 않음.`
`Box<Integer> intBox = BoxFactory.make();  // 가능!`

인자를 받지 않는 경우에도 별도로 Type을 표기해주지 않아도 된다! 이는 자바 7부터 컴파일러의 자료형 유추 범위가 넓어졌기 때문이다. 왼쪽 항에 선언된 참조변수의 Parameterized Type(`Box<Integer>`)를 참고하여, 반환 값의 Type Argument(`Integer`)를 판단할 수 있다. 이때 **유추에 사용된 참조변수의 Parameterized Type(`Box<Integer>`)를 타겟 타입(Target Type)**이라고 한다.

**제네릭 메서드도 제네릭 클래스와 같은 방식으로 Type Argument를 제한**할 수 있으며, 제한한 클래스의 메서드 또는 인터페이스의 메서드를 호출할 수 있다.

### 제네릭 클래스와 상속
다음과 같이 `Box<T>` 클래스를 상속하는 `PaperBox<T>`를 정의해보자

`class PaperBox<T> extends Box<T> {...}`

이때 두 클래스는 다음과 같은 상속관계를 형성한다. 즉, 당연히 `PaperBox<T>` 클래스는 `Box<T>` 클래스를 상속한다.

![](https://images.velog.io/images/harang/post/e193c89d-149b-4798-89de-3c7f9cf28e00/Generics%20Inherit.png)

다만, 주의해야할 점이 있다. 다음의 코드를 보자.

`Box<Number> box = new Box<Integer>();`

얼핏 보면 정상적으로 컴파일될 것 처럼 보인다만, 이는 컴파일 되지 않는다. 결론적으로 제네릭 클래스의 상속 관계는 Parameterized Type(제네릭 타입)의 상속 관계를 기준으로 따지고, Type Argument의 상속 관계는 유효하지 않다.

## 와일드카드(Wildcard)
와일드카드..? 필자는 처음 와일드카드를 접했을 때 케이팝스타의 와일드카드가 떠올랐다..
![](https://img2.sbs.co.kr/img/seditor/VD/2016/01/31/VD41415963_w640.jpg)
이미지 출처: https://programs.sbs.co.kr/enter/kpopstar5/vod/55429/22000160201

혹시 해당 글의 Java Wildcard와 연관성이 있을까하여 찾아봤으나, 무관한 걸로..

케이팝스타의 와일드카드는 잊어버리고, 다음의 메서드를 살펴보자

```java
public static <T> void peekBox(Box<T> box) {
	...
}
```

이 메서드를 제네릭으로 정의한 이유는 **`Box<Something>`의 인스턴스를 인자로 받기 위함**이다. 그렇다면, 이를 `public static void peekBox(Box<Object> box)`로 정의할 수 있을까? 불가능하다. **`Box<Object>`는 `Box<Something>`과 상속 관계를 형성하지 않기 때문**이다. 이 상황에 와일드카드를 사용하여 원하는 바를 이룰 수 있다. 와일드 카드를 사용하여 다시 메서드를 정의해보자.

```java
public static void peekBox(Box<?> box) {
	...
}
```

위의 코드라면, 원하는대로 `Box<Something>`을 인자로 받을 수 있다. 혹자는 이렇게 생각할 수도 있다. *어짜피 똑같은 기능을 수행하는 메서드인데, 왜 굳이 와일드카드를 써야하나?* 맞는 말이다. 그러나 **와일드카드를 사용한 코드가 더 깔끔하기 때문에, 보편적으로 와일드카드 사용을 선호**한다.

### 와일드 카드 상한/하한 제한(Bounded Wildcards)
#### 상한 제한(Upper-Bounded Wildcards)
`Box<? extends Number> box`
이 정의가 의미하는 바는 다음과 같다.

1. box는 `Box<T>` 인스턴스를 참조하는 참조변수 이다.
2. 이때 `Box<T>` 인스턴스의 T는 Number이거나 이를 상속하는 하위 클래스여야 한다.
3. `Box<T>`의 T를 Number 또는 Number를 직간접적으로 상속하는 하위 클래스로 제한한다.

#### 하한 제한(Lower-Bounded Wildcards)
`Box<? super Integer> box`
이 정의가 의미하는 바는 다음과 같다.

1. box는 `Box<T>` 인스턴스를 참조하는 참조변수 이다.
2. 이때 `Box<T>` 인스턴스의 T는 Integer이거나 이가 상속하는 상위 클래스여야 한다.
3. `Box<T>`의 T를 Integer 또는 Interger가 직간접적으로 상속하는 상위 클래스로 제한한다.

### 와일드카드 제한의 목적
앞서 제시한 상/하한 제한의 3번 사항을 살펴보자. 이는 정확한 설명이다. 인자로 전달되는 대상을 제한하는 것은 그 자체로 프로그램에 안정성을 높여준다. 그러나 다른 관점에서 상/하한 제한된 와일드카드의 의미를 이해할 수 있어야 한다. 이와 관련하여 다음의 코드를 살펴보자. 박스의 내용물을 빼내고 넣는 static 메서드이다.

```java
class Box<T> {
	private T o;
    
    public void set(T o) {
    	this.o = o;
    }
    
    public T get() {
    	return o;
    }
}

class BoxHandler {
	public static void out(Box<Number> box) {
    	Number t = box.get();  // Box에서 빼내기
        // box.set(new Number(1)); // Error
        System.out.println(t);
    }
    
    public static void in(Box<Number> box, Number n) {
    	box.set(n);  // Box에 넣기
        // Number num = box.get(); // Error
    }
}
```

두 메서드 모두 정상적으로 동작한다. 그러나 다음의 조건을 만족하지 않는다.
>"**필요한 만큼만 기능을 허용**하여, **코드의 오류가 컴파일 과정에서 최대한 발견되도록** 한다." (윤성우의 열혈 Java 프로그래밍)

즉, **out 메서드의 경우 꺼내는 기능(get)만 허용하도록, in 메서드의 경우 넣는 기능(set)만 허용하도록 하는 것이 바람직**하다. 프로그래머의 실수로 인해 주석 처리된 라인들이 각 메서드에 작성된다면, 오류가 발생할 뿐만 아니라 이 오류는 컴파일 과정에서 발견되지 않는다. 물론 와일드카드의 제한을 적절히 활용하여 이 상황을 해결할 수 있다.

#### 상한 제한의 목적
out 메서드(`public static void out(Box<Number> box)`) 다음과 같이 수정하여 out 메서드에서의 넣는 기능(set)을 제한할 수 있다.
 
 `public static void out(Box<? extends Number> box)`

이때 **set 메서드의 파라미터로 Number 인스턴스를 저장할 수 있는 Box만 전달된다는 사실을 보장할 수 없기 때문에**, set 메서드의 호출은 불가능하다. 즉, 위 메서드의 정의를 통해 다음과 같은 판단을 할 수 있어야 한다.

>box가 참조하는 인스턴스를 대상으로 저장하는 기능의 메서드 호출(set)은 불가능하다.

#### 하한 제한의 목적
마찬가지로 in 메서드(`public static void in(Box<Number> box, Number n)`) 다음과 같이 수정하여 out 메서드에서의 넣는 기능(set)을 제한할 수 있다.
 
 `public static void in(Box<? super Number> box, Number n)`

이때 in 메서드에서 get 메서드를 호출하는 라인을 보자. **get 메서드를 통해 반환된 값의 Type이 Number라는 사실을 보장할 수 없기 때문에, get 메서드의 호출문은 컴파일 에러를 발생**시킨다(반환 값의 참조변수 형을 Number로 결정했기 때문에). 즉, 위 메서드의 정의를 통해 다음과 같은 판단을 할 수 있어야 한다.

>box가 참조하는 인스턴스를 대상으로 꺼내는(반환하는) 기능의 메서드 호출(get)은 불가능하다.

### Type Erasure과 제네릭 메소드
먼저, 다음의 코드를 살펴보자
```java
class BoxHandler {
	public static void out(Box<? extends Number> box) {...}
    public static void out(Box<? extends Integer> box) {...}
}
```

위 클래스의 두 메서드 정의는 오버로딩이 성립하지 않는다. 자바는 제네릭 이전 클래스들과의 상호 호환성 유지를 위해 컴파일 시 제네릭과 와일드카드 관련 정보를 지우는 과정을 거친다. 이를 **Type Erasure**라고 한다. 이 때문에 두 파라미터의 선언은 컴파일 이후 다음과 같이 변한된다. 따라서, 위와 오버로딩은 컴파일 과정 중에 name clash 에러가 발생시킨다.

* `Box<? extends Number> box` -> `Box box`
* `Box<? extends Integer> box` -> `Box box`

이렇게 메소드 오버로딩이 성립되지 않는 경우, 제네릭 메소드로 정의해줌으로써 해결할 수 있다. 이는 다음과 같다.

`public static <T> void out(Box<? extends T> box) {...}`

## Reference
* [자바 제네릭 이해하기](https://yaboong.github.io/java/2019/01/19/java-generics-1/)
* [Java Documentation / Why Use Generics?](https://docs.oracle.com/javase/tutorial/java/generics/why.html)
* [Java Generics - No Static field](https://www.tutorialspoint.com/java_generics/java_generics_no_static.htm)
* 윤성우의 열혈 Java 프로그래밍(윤성우, 오렌지 미디어, 2017)