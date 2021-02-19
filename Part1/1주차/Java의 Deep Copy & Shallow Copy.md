# Java의 Deep Copy / Shallow Copy
*시작에 앞서, 이 글은 책, 강의, 구글링등을 통해 개인적으로 학습 및 실습한 개념을 정리한 글이며, 정확하지 않은 내용이 있을 수 있습니다. 꾸준히 공부해가며 수정하겠습니다. 또한, 오류 지적은 언제나 환영입니다.*


간단한 개념설명과 예제를 통해 쉽게 감을 잡을 수 있는 주제다. 개념에 대한 간단한 설명은 [이 글](https://marshallslee.tistory.com/entry/%EC%9E%90%EB%B0%94-Cloneable-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%EA%B7%B8%EB%A6%AC%EA%B3%A0-deep-copy-vs-shallow-copy)을 참고하시길 바란다. 거기까지만 하면 아쉬우니 직접 확인해보고자 한다. 다양한 예제를 통해 확인해보자!

## Primative Type
보통 Java의 깊은 복사, 얕은 복사를 생각하면 Object를 이용한 예시가 처음 떠오르는데, 그전에 먼저 Primative Type을 다루겠다. 결론부터 이야기하면 **Primative Type의 경우, 깊은 복사만 이루어진다.** `int` 타입을 예시로 들어 직접 확인해보자.
```java
int a = 2048;
int b = a;
System.out.println(a);  // 2048
System.out.println(b);  // 2048
System.out.println(a == b); // true

b = 1024;
System.out.println(a);  // 2048
System.out.println(b);  // 1024
System.out.println(a == b); // false
```
일단 위의 코드를 보고나서 사본의 값을 변경했을 때 원본은 진짜 깊은 복사가 이뤄지는 구만.. 이라고 생각했지만, 초기 `a==b`의 출력값이 `true`라고..? **깊은 복사가 이뤄진다면 `a`의 `2048`과 `b`의 `2048`이 달라야하는 것 아닌가..** 라고 생각할 수도 있지만..!(의식의 흐름대로 썼습니다 ㅠ) 원래 Java의 `==` 연산자는 값을 비교하기 때문에 이는 깊은 복사와 무관하다. 2개의 변수를 선언했지만, 깊은 복사가 제대로 이루어졌다면 `main()` method에 대한 Local Variable Array의 최대 length는 3이 되어야 할 것이다. 예제 Source Code의 Bytecode를 통해 이를 확인해보았다. 일단 `main()` Method에 대한 Bytecode는 다음과 같다.(출력 코드는 제외 / [Opcode 참고](https://javaalmanac.io/bytecode/))
```java
...
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   ...
{
...
  public static void main(java.lang.String[]);
    ...
      stack=1, locals=3, args_size=1
         ...
      LineNumberTable:
        ...
}
SourceFile: "DeepCopy.java"
```
위의 Bytecode를 통해 2가지 사실을 확인할 수 있다. 첫 번째로 `locals=3`(`a`의 `2048`, `b`의 `2048`, `b`의 `1024`)이므로 **깊은 복사가 제대로 이루어 졌음을 확인할 수 있다.** 또, 코드는 생략했지만, Runtime Constant Pool에 `int` 리터럴 값이 존재하지 않는 걸로 보아, Primative Type의 깊은 복사는 모두 Stack 영역에서 진행되었음을 알 수 있다.

## Object
이제 Object를 살펴보자. 설명하기 전에, 간단한 퀴즈를 하나 소개하겠다.

```java
String name1 = "Deep";
String name2 = "Deep";
String name3 = new String(name1);
String name4 = new String("Deep");
 
System.out.println(name1 == name2); // ?
System.out.println(name1 == name3); // ?
System.out.println(name1 == name4); // ?
System.out.println(name1 == "Deep"); // ?
```

이미 잘 알고 있다면, 굿이다. 헷갈린다면, 이 글을 잘 읽어보자. 그 후에는 **명확하게** 알 수 있을 것이다.

### Shallow Copy
먼저 얕은 복사를 살펴보자. 예제 코드는 다음과 같다.
```java
Cop copy1 = new Cop(1);
Cop copy2 = copy1;

System.out.println(copy1.getValue());   // 1
System.out.println(copy2.getValue());   // 1
System.out.println(copy1 == copy2);   // true

copy2.setValue(2);

System.out.println(copy1.getValue());   // 2
System.out.println(copy2.getValue());   // 2
System.out.println(copy1 == copy2);   // true
```
한 줄씩 코드를 읽어가며, 그 과정을 따라가보자.

* `Cop copy1 = new Cop(1);`
  Heap 영역에 새로운 `Cop` 객체를 생성하고, address를 `Cop` 참조형 변수 `copy1` 대입한다. 따라서 `copy1` 새로 생성된 객체를 가리키며, 이는 다음 그림과 같다.
  
  ![](https://images.velog.io/images/harang/post/e77b3db5-8577-4699-965b-3d4b2a3c2746/Shallow%20Copy.png)

* `Cop copy2 = copy1;`
  `copy1`의 값, 즉 이전 단계에서 생성된 `Cop` 인스턴스의 address를 `copy2`에 대입한다. 따라서 `copy2` 역시 같은 객체를 가리키고, 이는 다음 그림과 같다.

  ![](https://images.velog.io/images/harang/post/4e7b1662-7bc7-4717-9792-75a05c96f590/Shallow%20Copy2.png)
 
* `copy2.setValue(2);`
  `copy2`가 가리키는 `Cop` 인스턴스의 `value`를 `2`로 변경한다. 물론 이는 `copy1`이 가리키는 객체이기도 하다. 따라서, **얕은 복사가 일어났음**을 알 수 있고, 이는 다음 그림과 같다.

  ![](https://images.velog.io/images/harang/post/60ebbcf0-d583-4411-ab04-895000a14ea4/Shallow%20Copy3.png)
  
Bytecode를 통해 그 과정을 실제 JVM의 동작을 직접 확인해보자.

```java
...
Constant pool:
  ...
   #7 = Class              #8             // staycozyboy/java/copy/Cop
   #8 = Utf8               staycozyboy/java/copy/Cop
   #9 = Methodref          #7.#10         // staycozyboy/java/copy/Cop."<init>":(I)V
  #10 = NameAndType        #5:#11         // "<init>":(I)V
  #11 = Utf8               (I)V
  ...
         0: new           #7                  // class staycozyboy/java/copy/Cop
         3: dup
         4: iconst_1
         5: invokespecial #9                  // Method staycozyboy/java/copy/Cop."<init>":(I)V
         8: astore_1
         9: aload_1
         ...
```
Bytecode를 확인보면 실제 Heap 영역에 **`Cop` 인스턴스가 한 개 생성되었으며, 생성자가 또한 딱 한번 호출되었음을 알 수 있다.** 따라서, 얕은 복사가 일어났음을 직접 확인할 수 있다.

### Deep Copy
얕은 복사와 마찬가지로 예시를 통해 깊은 복사가 이뤄지는 과정을 직접 확인해보자.

#### `clone` Method
Object class에는 instance의 복사를 위한 다음 method가 정의되어 있다.
`protected Object clone() throws CloneNotSupportedException`
이 method가 호출되면, 호출된 메소드가 속한 instance의 사본이 생성되고, 이의 address 값을 반환한다. 이때, 이는 다음의 interface를 구현한 class의 instance를 대상으로만 위의 method를 호출할 수 있다.
`interface Clonable`
(참고로, Clonable는 Marker Interface다. 즉, 정의해야할 메소드가 존재하지 않고, instance 복사가 가능하다는 marking의 interface다.)

그럼, `clone` method를 사용해 본격적으로 Deep Copy를 해보자.

#### Object deep copy 기본 예제
```java
Cop copy1 = new Cop(1);
Cop copy2;


try{
    copy2 = (Cop) copy1.clone();
    System.out.println(copy1.getValue());   // 1
    System.out.println(copy2.getValue());   // 1
    System.out.println(copy1 == copy2);   // false
    copy2.setValue(2);
    System.out.println(copy1.getValue());   // 1
    System.out.println(copy2.getValue());   // 2
}
catch(CloneNotSupportedException e) {
    e.printStackTrace();
}
```
참고로, 기존에는 예제 코드처럼 `clone` method의 반환 형이 `Object`이므로 강제 Casting을 해야했다. 하지만, Java 5 이후부터 method overriding 시 caller의 class로 수정하는 경우에 한해 return type의 수정을 허용하기 때문에 **Casting없이도 `clone`의 호출이 가능하다(하지만, 당연히 return하기 전에 caller의 class로 Casting해서 return 해줘야함).**  또한, `clone` method는 protected로 선언되어 있기 때문에, 꼭 overriding하여 public으로 접근 범위를 넓혀줘야 한다. 이번에도 마찬가지로 코드를 한 줄씩 읽어가며, 과정을 확인해보자.

* `Cop copy1 = new Cop(1);`
  앞서 [얕은 복사](#shallow-copy)에서 설명한 과정과 똑같기 때문에, 설명은 생략한다.

  ![](https://images.velog.io/images/harang/post/e77b3db5-8577-4699-965b-3d4b2a3c2746/Shallow%20Copy.png)

* `Cop copy2;`
  `copy2`의 선언. 기본값인 `null` 값을 가진다.

  ![](https://images.velog.io/images/harang/post/29b42703-62f1-4b15-aba8-9247f858a65a/Deep%20Copy.png)

* `copy2 = (Cop) copy1.clone();`
 `copy1`이 가리키고 있는 **`Cop` 인스턴스의 복사본을 새로 생성**하고, 이의 address 값을 `copy2`에 대입한다.

  ![](https://images.velog.io/images/harang/post/b6bccc1d-67c9-4914-81e4-63ca470e4b0b/Deep%20Copy2.png)

* `copy2.setValue(2);`
  `copy2`가 가리키고 있는 `Cop` 인스턴스의 `value`를 `2`로 바꾼다. 이때, `copy1`이 가리키는 인스턴스와 `copy2`가 가리키는 인스턴스는 서로 다르기 때문에, `copy1`의 인스턴스의 `value` 값은 변하지 않는다. **이를 통해 깊은 복사가 일어났음**을 알 수 있다.
  
  ![](https://images.velog.io/images/harang/post/b1317bd3-51fb-493e-86cb-4eca5132fcae/Deep%20Copy3.png)
  
얕은 복사와 마찬가지로 Bytecode를 통해 실제 JVM의 동작을 직접 확인해보자.
```
...
...
Constant pool:
  ...
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // staycozyboy/java/copy/Cop
   #8 = Utf8               staycozyboy/java/copy/Cop
   #9 = Methodref          #7.#10         // staycozyboy/java/copy/Cop."<init>":(I)V
  #10 = NameAndType        #5:#11         // "<init>":(I)V
  #11 = Utf8               (I)V
  #12 = Methodref          #7.#13         // staycozyboy/java/copy/Cop.clone:()Ljava/lang/Object;
  #13 = NameAndType        #14:#15        // clone:()Ljava/lang/Object;
  #14 = Utf8               clone
  #15 = Utf8               ()Ljava/lang/Object;
  ...
         0: new           #7                  // class staycozyboy/java/copy/Cop
         3: dup
         4: iconst_1
         5: invokespecial #9                  // Method staycozyboy/java/copy/Cop."<init>":(I)V
         8: astore_1
         9: aload_1
        10: invokevirtual #12                 // Method staycozyboy/java/copy/Cop.clone:()Ljava/lang/Object;
        13: checkcast     #7                  // class staycozyboy/java/copy/Cop
        16: astore_2
        17: aload_2
        ...
```
마찬가지로, Bytecode를 확인해보면 실제로 **`Cop` 객체가 2개 생성**되었으며, 생성자와 `clone` method가 각각 한 번씩 호출되었음을 확인할 수 있다. 따라서, **깊은 복사가 일어났음**을 알 수 있다. 

이로써 Bytecode level에서 실제 JVM 동작까지 직접 확인하였고, 얕은 복사와 깊은 복사에 대해 알아보았다.


## 추가 예제
사실 여기까지 잘 이해했다면, 두 개념에 대해 확실히 감을 잡았을 것이다. 추가적으로 헷갈릴 수 있는 재밌는 예제를 몇가지 다루어보겠다.

### Array의 복사
Java 입문자라면, Array의 경우 Primative Type은 아니고.. 그렇다고 Object도 아닌것 같은데.. 이 친구는 복사가 어떻게 진행되는거지? 라는 의문이 생길 수도 있다(내가 생겨서 알아봤다.. ㅎㅅㅎ). 간단하지만 헷갈릴 수 있는 부분이라고 생각하여 정리한다. 자세한 내용은 [stackoverflow](https://stackoverflow.com/questions/12757841/are-arrays-passed-by-value-or-passed-by-reference-in-java)를 참고하자. 결론적으로 해답은 다음과 같다.

1. **Array는 Object다.** (`java.lang.Object`를 부모 클래스로 가지며, `java.lang.Object` API의 모든 method의 implementation을 상속한다.)
2. 따라서, Array의 복사는 Object와 동일하게 진행된다.

### String Constant Pool
처음 단원을 시작할 때 소개한 퀴즈를 풀었는가? 이제 정답을 공개하겠다.

```java
String name1 = "Deep";
String name2 = "Deep";
String name3 = new String(name1);
String name4 = new String("Deep");
 
System.out.println(name1 == name2); // true
System.out.println(name1 == name3); // false
System.out.println(name1 == name4); // false
System.out.println(name1 == "Deep"); // true
```
이 퀴즈의 실마리는 String Constant Pool에 있다. 코드를 한 줄씩 읽어가며 간략하게 핵심 내용만 짚어 설명하겠다. 자세한 내용은 [String Constant Pool](https://www.geeksforgeeks.org/string-constant-pool-in-java/)을 참고하자.

1. String Constant Pool은 Heap 영역 내에 존재하는 small cache다.
2. Direct allocation의 경우(`String name1 = "Deep";`), String 인스턴스는 String Constant Pool에 저장된다.
  ![](https://images.velog.io/images/harang/post/714c6507-46ab-4e10-873e-398f0a017dbc/String%20Constant%20Pool.png)
3. String Constant Pool에 이미 저장된 String과 같은 값을 가지는 String 인스턴스를 Direct allocation하는 경우(`String name2 = "Deep";`), **"Deep"의 값을 가지는 새로운 String 인스턴스는 생성되지 않는다.** 그 대신 이미 저장된 "Deep"의 값을 재사용한다. 즉, 다음 그림과 같다.
  ![](https://images.velog.io/images/harang/post/95f33feb-ce12-491d-a371-63e5411be0e2/String%20Constant%20Pool2.png)
4. 즉, String Constant Pool의 목적은 이런식으로 memory상에 이미 저장된 인스턴스를 재사용하여 **불필요하게** 생성되는 인스턴스를 최소화하기 위함이다. 이를 통해 **memory usage를 줄이고, 성능은 향상**시킬 수 있다.
5. 이때, 같은 값을 가지는 String 인스턴스가 오직 하나만 필요한 이유는 무엇일까? 이는 **String 인스턴스가 Immutuable 인스턴스**이기 때문이다.
6. 마지막으로, `new String()`을 통해 인스턴스를 생성하는 경우(`name3`와 `name4`), 일반적인 Object 생성과 똑같이 수행된다. 즉, **Heap 영역 중에서 String Constant Pool이 아닌 영역에 저장된다**는 뜻이다. 따라서 `name3`과 `name4`의 초기화 이후의 Memory 영역은 다음과 같다.
  ![](https://images.velog.io/images/harang/post/a758fa7a-fc0c-43a1-b427-c398238c0d38/String%20Constant%20Pool4.png)

## 마무리

개인적으로 이번 Topic을 공부하며, 역시 C를 제대로 공부해야함을 절실히 느꼈다. 특히 pass-by-value & pass-by referece를 공부하며 질문이 엄청 생겼는데, C를 깊게 알지 못하니 궁금증을 해결할 수 없었다. 역시 개발자는 C를 해야 돼,, 계획한 것 보다 더 일찍 C 공부를 시작해야겠다.

## References
* [Clonable Interface & Deep Copy / Shallow Copy](https://marshallslee.tistory.com/entry/%EC%9E%90%EB%B0%94-Cloneable-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%EA%B7%B8%EB%A6%AC%EA%B3%A0-deep-copy-vs-shallow-copy)
* [javaalmanac.io/Java Bytecode](https://javaalmanac.io/bytecode/)
* [Are arrays passed by value or passed by reference in Java?](https://stackoverflow.com/questions/12757841/are-arrays-passed-by-value-or-passed-by-reference-in-java)
* [Pass by Reference vs. Pass by Value](https://www.cs.fsu.edu/~myers/c++/notes/references.html)
* [Is Java "pass-by-reference" or "pass-by-value"?](https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value)
* [Java is Pass-by-Value, Dammit!](http://www.javadude.com/articles/passbyvalue.htm)
* [Formal parameter, Actual parameter & parameter passing](https://yunmap.tistory.com/entry/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%96%B8%EC%96%B4-Formal-parameter-Actual-parameter-%EA%B7%B8%EB%A6%AC%EA%B3%A0-parameter-passing)
* [C++ Reference](https://www.cprogramming.com/tutorial/references.html)
* [Are Reference and Address same?](http://www.cplusplus.com/forum/general/139756/)
* [Java Deep Copy & Shallow Copy](https://yyman.tistory.com/529)
* [String Constant Pool in Java](https://www.geeksforgeeks.org/string-constant-pool-in-java/)
* [String.intern in Java 6, 7 and 8 - string pooling](http://java-performance.info/string-intern-in-java-6-7-8/)
* [Java Compile to Run](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/)
* [Object Pool Design Pattern](https://www.geeksforgeeks.org/object-pool-design-pattern/?ref=rp)
* 윤성우의 열혈 Java 프로그래밍(윤성우, 오렌지 미디어, 2017)