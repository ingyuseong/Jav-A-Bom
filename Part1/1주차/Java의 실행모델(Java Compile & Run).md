# Java의 실행모델
*시작에 앞서, 이 글은 책, 강의, 구글링등을 통해 개인적으로 학습 및 실습한 개념을 정리한 글이며, 정확하지 않은 내용이 있을 수 있습니다. 꾸준히 공부해가며 수정하겠습니다. 또한, 오류 지적은 언제나 환영입니다.*

## Java 실행 모델의 큰 그림
일단 세부적인 구동원리는 제쳐두고, 개략적으로 필수적인 사항만 살펴봄으로써 Java 실행 모델에 대한 감을 잡아보자.


### Java Program의 실행 구조와 JVM
일반적인 프로그램은 운영체제 위에서 실행된다. 그 구조는 아래와 같다.
![](https://images.velog.io/images/harang/post/96fb8e1d-9d76-4e81-8de7-c4be4a5fc19d/Untitled%20Diagram.jpg)
HW를 기반으로 OS가 동작하고, 그 위에서 Program이 실행되는 구조다. 즉, HW 위에서 실행되는 **OS가 Program을 실행시키는 구조**이다. 하지만, Java Program의 실행 구조는 아래와 같다.
![](https://images.velog.io/images/harang/post/17cde658-0cf7-49dd-9e43-b7bf61cde76e/2.jpg)
JVM은 OS 위에서 실행되고, **Java Program(정확히는 바이트코드 / \*.class)은 JVM(정확히는 JRE. 핵심 요소가 JVM.) 상에서 실행**된다. 왜 Java Program은 OS 위에서 직접 실행하는 것이 아닌 JVM 상에서 실행하도록 설계한 것일까? 이는 **Java Program을 OS에 상관없이 실행시키기 위함**이다. 일반 Program의 경우, 같은 기능을 수행하는 Program이라도 OS에 따라 다르게 구현해야 한다. But, Java Program의 경우 아래 그림과 같이 코드 수정 없이 다양한 OS 상에서 실행 가능하다.
![](https://images.velog.io/images/harang/post/c2b3e749-eed7-4832-be7c-28f1200952f2/4.png)
여기서 주목할 점은 Windows 위에서 실행되는 JVM과 Linux 위에서 실행되는 JVM이 다르다는 점이다. 따라서, **각 OS에 맞는 JVM을 설치해야 OS에 상관없이 Java Program을 실행**시킬 수 있다. 그럼 Java는 cross-platform인가? 앞서 내용을 살펴봤다면 쉽게 답을 할 수 있겠지만, 표현에 따라 답이 굉장히 애매해질 수 있다. 따라서, Yes or No로 답하기 보다는 **이에 대한 팩트를 정확히 알고 있느냐**가 중요하다고 생각한다.

### Java는 정말 cross-platform?
팩트만 짚고 넘어가겠다.

1. JVM은 Java Bytecode를 실행함. 즉, **Java Bytecode는 플랫폼 의존적인 코드가 없음**.
2. 따라서, **Java Program의 실행은 OS나 Device의 영향을 받지 않음**.
3. 허나, **OS나 Device에 맞는 JVM이 설치되어 있지 않다면 Java Program 실행 불가능**.

## Java Program의 실행 과정
Java 11 JVM 스펙을 기준으로 **Java Source Code가 어떤 과정을 거쳐 실행되는지** 알아보자. 사실 깊게 팔려면 더 깊게 팔 수도 있고, 안그래도 내용이 워낙 많아서 어디까지 포함하여 정리할 지 많이 고민했다. 핵심 흐름을 중심으로 정리했고, 너무 지엽적인 내용은 제외했다. **딱 흐름을 정확하게 이해하기 위해 필요한 수준**을 기준으로 정리했다.

이번에도 개략적인 모델을 먼저 살펴보자. 이는 다음와 같다.
![](https://images.velog.io/images/harang/post/1df4d89a-de5e-4766-9835-f79ab7e7f368/5.png)

위의 그림에서 알 수 있듯이 Java Source Code가 실행되기까지의 과정은 다음과 같이 정리할 수 있다.

1. Java Complier(javac.exe)를 사용해 Java Source Code(\*.java)를 바탕으로 Java Bytecode(\*.class)을 만든다**(Compile!)**.
2. JRE의 java.exe를 사용해 Java Bytecode를 실행한다**(Run!)**.
3. JVM이 동작하며 Java Bytecode를 실질적으로 실행(메모리 할당, 시스템 명령 호출 등)한다.

요약하자면, 아래와 같다.

1. **Compile!**
2. **Run!**

매우 간단하다. 하지만 이대로 끝내기 아쉬우니, 이 두 가지를 조금만 더 자세히 알아보자.

### Compile!
Java에서의 Compile은 전통적인 Compile과는 조금 다르다. 먼저, 전통적인 Compile은 인간이 이해하기 쉬운 **고수준 언어(C, C++, C# 등)**로 작성된 Source Code를 컴퓨터가 이해할 수 있는 **기계어**, 즉 Native Code로 변환하는 것을 의미한다. 하지만, 흔히 Java에서의 Compile이라고 하면 **Java로 작성된 Source Code**를 **JVM이 실행할 수 있는 Bytecode**로 변환하는 것을 의미한다.

* C Compile(전통적인 Compile) : C Source Code(.c, .h) -> Machine Code(.exe, .out) 실행파일
* Java Compile : Java Source Code(.java) -> Bytecode(.class)

즉, C의 경우(전통적인 Compile) Complie 이후, **실행파일(.exe, .out)**을 얻는다(정확히는 전처리/컴파일/어셈블리/링크의 과정을 거쳐 최종 실행파일이 생성됨. 흔히 컴파일이라 하면 컴파일+어셈블리의 과정을 의미함. [참고](https://www.geeksforgeeks.org/compiling-a-c-program-behind-the-scenes/)). 이때 실행파일은 기계어이며, OS가 바로 실행 가능한 파일이다. 하지만, Java의 경우 Compile 이후, **실행 파일이 아닌 Bytecode(.class)**를 얻는다. Bytecode는 **특정 OS/Device가 이해하는 기계어가 아니며**, 실행 과정에서 **JVM이 적절한  Native Code로 변환하여 실행**한다. 당연히 OS가 최종 실행파일을 직접 실행하는 것이 JVM이 Bytecode를 실행하는 것보다 빠르다. 이는 [실행 부분](#run)에서 자세히 다룰 것이다.

정리하자면, Java의 Compile은 **Java Source Code를 Java 언어 스펙에 따라 분석 & 검증하고, JVM 스펙에 맞는 Bytecode를 만들어 내는 과정**이다.

참고로 이는 JVM 스펙에 맞는 Bytecode를 만들어 낼 수 있다면, Java가 아닌 다른 언어(예를들어 Kotlin)도 JVM 위에서 실행가능함을 의미하기도 한다.

### Run!
앞서 언급하였듯이 java.exe(`java` command)를 사용해 Java application을 실행한다. Java application 실행 과정은 다음과 같다([앞서 제시한 실행 과정의 개략적인 모델 참고](#java-program의-실행-과정)).

1. `java` command에 의해 JVM이 실행된다.
2. Class Loader가 Bytecode를 Runtime Data Areas에 `load`한다. 이때, `load`는 [다음](#class-load)과 같은 과정이다.
3. Execution Engine이 Bytecode를 실행한다.

[Oracle Tools Reference](https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE)에서는 `java` command를 다음와 같이 설명하고 있다.

1. `java` command는 Java application을 시작한다.
2. 1번 작업은 JRE를 시작하고, `java` command의 **인자로 지정된 class(initial class)를 로딩**하고, 그 class의 **`main()` method를 호출함**으로써 수행한다. 이때, 이 method의 선언은 다음의 형식을 따른다 : `public static void main(String[] args)`

위의 두 가지 설명을 참고하여 정리해보자. `java` command는 다음과 같이 Java application을 시작한다.

1. JVM을 실행한다.
2. Class Loader를 사용해 initial class를 `load`한다.
3. `main()` method를 호출한다.

예제를 통해 위의 과정을 자세히 살펴보자. 흐름을 이해하기 위해 알아야할 필수 개념들은 흐름을 따라가며 다루겠다.

해당 글에서 쓰인 각 용어에 대한 의미는 [JVM Specification의 Chapter 5](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-5.html)를 참고.

#### 예제 소스 코드
설명이 굳이 필요없는 매우 간단한 코드다. `Aloha` 객체를 생성하고, 인스턴스 method를 호출하여 `"Aloooha!"`를 출력하는 코드다.
```java
package staycozyboy.java.jvm;

public class Aloha {
    public static void main(String[] args) {
        final Aloha aloha = new Aloha();
        System.out.println(aloha.aloooha());
    }

    private String aloooha() {
        return "Aloooha!";
    }
}
```
#### 예제 바이트 코드
아래의 Code는 Source Code를 Compile하여 얻은 Bytecode다. **필요한 부분만 짚어서 다룰 것이기 때문에, 그냥 대충 이렇게 생겼구나 정도 확인하고 넘어가면 된다.** 참고로 `javap` command로 Bytecode를 확인할 수 있다. 이는 Binary 파일인 Class 파일을 텍스트로 보여주는 역어셈블러(disassembler)다. 이를 이용한 결과물을 흔히 자바 어셈블리라고 부른다. 자세한 사항은 [Orcale Tools Reference](https://docs.oracle.com/en/java/javase/11/tools/javap.html#GUID-BE20562C-912A-4F91-85CF-24909F212D7F) 참고.
> `javap -v -l -p class\staycozyboy\java\jvm\Aloha.class`

```java
Classfile /C:/Users/PC/java_test/Hello/class/staycozyboy/java/jvm/Aloha.class
  Last modified 2020. 12. 24.; size 536 bytes
  SHA-256 checksum da4341273ecc0e3698bf715fe440ae48c45f57d8c0c4691b9b51dd8a7e5ce236
  Compiled from "Aloha.java"
public class staycozyboy.java.jvm.Aloha
  minor version: 0
  major version: 59
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #7                          // staycozyboy/java/jvm/Aloha
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 3, attributes: 1
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // staycozyboy/java/jvm/Aloha
   #8 = Utf8               staycozyboy/java/jvm/Aloha
   #9 = Methodref          #7.#3          // staycozyboy/java/jvm/Aloha."<init>":()V
  #10 = Fieldref           #11.#12        // java/lang/System.out:Ljava/io/PrintStream;
  #11 = Class              #13            // java/lang/System
  #12 = NameAndType        #14:#15        // out:Ljava/io/PrintStream;
  #13 = Utf8               java/lang/System
  #14 = Utf8               out
  #15 = Utf8               Ljava/io/PrintStream;
  #16 = Methodref          #7.#17         // staycozyboy/java/jvm/Aloha.aloooha:()Ljava/lang/String;
  #17 = NameAndType        #18:#19        // aloooha:()Ljava/lang/String;
  #18 = Utf8               aloooha
  #19 = Utf8               ()Ljava/lang/String;
  #20 = Methodref          #21.#22        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #21 = Class              #23            // java/io/PrintStream
  #22 = NameAndType        #24:#25        // println:(Ljava/lang/String;)V
  #23 = Utf8               java/io/PrintStream
  #24 = Utf8               println
  #25 = Utf8               (Ljava/lang/String;)V
  #26 = String             #27            // Aloooha!
  #27 = Utf8               Aloooha!
  #28 = Utf8               Code
  #29 = Utf8               LineNumberTable
  #30 = Utf8               main
  #31 = Utf8               ([Ljava/lang/String;)V
  #32 = Utf8               SourceFile
  #33 = Utf8               Aloha.java
{
  public staycozyboy.java.jvm.Aloha();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=2, args_size=1
         0: new           #7                  // class staycozyboy/java/jvm/Aloha
         3: dup
         4: invokespecial #9                  // Method "<init>":()V
         7: astore_1
         8: getstatic     #10                 // Field java/lang/System.out:Ljava/io/PrintStream;
        11: aload_1
        12: invokevirtual #16                 // Method aloooha:()Ljava/lang/String;
        15: invokevirtual #20                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        18: return
      LineNumberTable:
        line 5: 0
        line 6: 8
        line 7: 18

  private java.lang.String aloooha();
    descriptor: ()Ljava/lang/String;
    flags: (0x0002) ACC_PRIVATE
    Code:
      stack=1, locals=1, args_size=1
         0: ldc           #26                 // String Aloooha!
         2: areturn
      LineNumberTable:
        line 10: 0
}
SourceFile: "Aloha.java"
```

#### 런타임 데이터 영역(Runtime Data Areas)
먼저, 흐름을 시작하기 전에 런타임 데이터 영역에 대해 간략하게 알아보자. (이걸 알아야 시작이 된다...) **런타임 데이터 영역은 JVM이 OS에게 할당받는 메모리 영역**이다. ~~(Main Memory의 효율적인 사용을 위해서 OS가 Memory를 관리하는데, 이는 OS가 프로그램에게 Memory를 할당한다는 의미이다. [앞서 제시한 그림](#java-program의-실행-구조와-jvm)을 보면 OS 입장에서는 JVM은 OS 상에서 실행되는 수많은 프로그램 중 하나일 뿐이다.)~~

런타임 데이터 영역은 6개 영역으로 나눌 수 있으며, 이는 다음과 같은 구조이다.
![](https://images.velog.io/images/harang/post/7c9752f2-31ad-405e-835d-99138363d55d/6.png)


1. PC Register
2. JVM Stack
3. Native Method Stack
4. Heap
5. Method Area
6. Runtime Constant Pool

여기서 `per-Class`, `per-Thread`등의 **단위는 생명 주기와 생성 단위**를 의미한다. 즉, `per-x`에 속하는 데이터 영역의 경우, `x`가 생성 & 소멸될 때 함께 생성 & 소멸되며, 당연히 `x`와 해당 데이터 영역은 1대1 대응된다. 이 정도만 알아두고 이제 1번 과정부터 설명을 시작해보자. 각 영역에 대한 **디테일한 설명은 자연스러운 흐름에 따라** 다룰 것이다.

#### JVM 실행
***
*1. JVM을 실행한다.* 의 과정이다. JVM이 실행되면 `per-JVM` 단위에 속하는 **Heap과 Method Area가 함께 생성**된다.

#### 힙 영역(Heap)
**인스턴스 또는 객체를 저장하는 영역으로 GC 대상**이다. Heap에 저장된 객체에 할당된 메모리는 명시적인 방법으로는 절대 회수할 수 없으며(권장되진 않지만, `System`의 `gc()` method를 통해 GC 수행을 **요청**할 수는 있다.), 오직 GC에 의해서만 회수될 수 있다. **JVM 성능 등의 이슈에서 가장 많이 언급되며, 모든 JVM 스레드에 공유**된다.

JVM이 막 실행된 이 시점에서 당연히 Heap은 빈 상태이다.

더 상세한 Heap 영역의 구조와 GC 알고리즘이 궁금하다면, https://d2.naver.com/helloworld/1329 참고.

#### 메소드 영역(Method Area)
Method Area 역시 모든 Thread가 공유하는 영역으로, JVM이 읽어 들인 각각의 **Class와 Interface에 대한 Runtime Constant Pool, 필드와 method 데이터, static 변수, method의 Bytecode를 저장**한다. 간단히 말하면, **거의 모든 Bytecode가 Method Area에 저장**된다고 보면 된다(거의라고 언급한 이유는 Bytecode에는 Constant Pool이 포함되어 있지만, Method Area에는 그를 바탕으로 Runtime Constant Pool을 만들어 저장하기 때문이다.).

JVM이 막 실행된 이 시점에서 당연히 Method Area 또한 빈 상태이다.

#### Initial Class `load` (1)
예제에서 initial class는 `Aloah` 이므로, 이 시점에서 **Class Loader는 `Aloah`를 `load`** 한다. 여기서 class `load`한다는 것의 의미와 Runtime Constant Pool에 대해 정확히 알아본 뒤, 다시 흐름을 진행하도록 하자. 

#### Class `load`
Java는 동적 `load`, **즉 Runtime에 Class를 처음으로 참조할 때 이를 로드하는 특징**이 있다. 이를 수행하는 부분이 JVM의 Class Loader이다. 자세한 내용은 [Java 클래스로더 훑어보기](https://homoefficio.github.io/2018/10/13/Java-%ED%81%B4%EB%9E%98%EC%8A%A4%EB%A1%9C%EB%8D%94-%ED%9B%91%EC%96%B4%EB%B3%B4%EA%B8%B0/)를 참고하자. 이 글에서는 Class Loader에 의해 수행되는 Class `load`의 개념에 대해서만 다루겠다. Class Loader가 아직 `load`되지 않은 Class를 찾으면, 다음과 같은 과정을 거쳐 `load`한다. 순서대로 살펴보자.
![](https://images.velog.io/images/harang/post/ee54a1ae-f487-47d6-9ef6-e7f470069ea7/7.png)

1. Loading : class(or interface)의 binary 표현을 찾아 이를 토대로 **class를 생성하는 것**. 이때, 생성은 class의 **Bytecode를 로딩하여 Method Area에 construction하는 것**을 의미한다(메모리 할당).
* Linking : **class의 상위 superclass, 또는 배열일 경우 배열의 원소인 class를 verifying / preparing / resolving 하는 과정.** 이때 class는 완전히 Loading된 후에 verifying & preparing을 수행하며, verifying & preparing을 완전히 수행한 이후 initializing을 수행한다. Resolving은 JVM에 따라 다르게 구현되어 있을 수 있으며, 동적으로 실행되기에 initializing 이후에 수행될 수도 있다.
2. Verifying : 읽어 들인 class가 Java Language & JVM Specification에 따라 **구조적으로 올바른지 검사하는 것.**
3. Preparing : **class의 static 필드를 생성하고 기본값으로 초기화하는 것**이다. 이때 기본값은 자료형에 따라 [JVM Specification](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-2.html#jvms-2.3)에 정의되어 있으며, 기본값이 아닌 특정값으로 초기화하는 과정은 Initializing 단계에서 수행된다.
4. Resolving : **Runtime Constant Pool의 Symbolic Reference를 동적으로 Direct Reference로 변경**하는 것이다. 
5. Initializing : Bytecode에서 `<clinit>`으로 표기된 class(or interface) initialization method([JVM Specification 참고](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-5.html#jvms-5.5))를 실행하는 것. 즉, static initializer block들을 실행하여, **static 필드를 적절한 값으로 초기화하는 것.**

#### 런타임 상수 풀(Runtime Constant Pool)
이는 `per-class` 단위에 속하므로 **class 생성시 생성되며, Bytecode에서 constant_pool 테이블에 해당하는 영역**이다. 이는 **컴파일타임에 이미 알 수 있는 리터럴 값뿐만 아니라, 런타임에 resolving되는 method와 필드에 대한 모든 reference를 담고 있는 테이블**이다. 즉, 특정 method 혹은 필드를 참조할 때 JVM은 Runtime Constant Pool을 통해 실제 Memory상 주소를 찾아 참조한다.

#### Initial Class `load` (2)
다시 Class Loader가 initial class인 `Aloah`를 `load`하는 시점으로 돌아와, 예제를 살펴보자.

1. Loading 과정을 수행하여 `Aloah`의 Bytecode가 Method Area에 저장되고, 이에 따라 Runtime Constant Pool도 함께 생성된다.
2. Linking 과정 중 Verifying이 수행되며, Aloah.class는 Java Compiler에 의해 정상적으로 Compile되었기 때문에, `Aloah`의 superclass인 `Object`가 로딩된다.
3. Linking 과정 중 Preparing이 수행되며, 원래는 static 필드의 생성이 이루어지지만, 예제의 경우 static 필드가 존재하지 않는다.
4. Linking 과정 중 Resolving의 경우, 원래는 동적으로 수행될 수 있다. 하지만, 설명의 편의를 위해 class에 대한 Verifying 수행되고 난 뒤, 즉시 그의 모든 Symbolic Reference에 대해 Resolving을 수행한다고 가정하자. 이 가정하에 Bytecode의 일부를 살펴봄으로써 Symbolic Reference가 Direct Reference로 변경되는 과정을 살펴보자.
    * `#7 = Class #8 // staycozyboy/java/jvm/Aloha`
  `Aloah` 인스턴스를 만들 때 필요한 Aloah Class 정보는 Method Area에 Loading되어 있으므로, Method Area 내에서 Aloah Class의 위치를 `Class staycozyboy/java/jvm/Aloha`의 값으로 Resolve할 수 있다.
    * `#10 = Fieldref #11.#12 // java/lang/System.out:Ljava/io/PrintStream;`
  `System` class는 로딩되어 있지 않기 때문에 이를 로딩한다. 이후 Verifying & Preparing 과정을 수행하며, `System`의 static 필드인 `out`은 기본값으로 초기화된다. 또, `out`은 `PrintStream` class의 인스턴스이기 때문에, `PrintStream` 클래스도 마찬가지로 로딩한다. **이런 Loading&Linking을 연쇄적으로 수행하며, Runtime Constant Pool 내의 Symbolic Reference를 Direct Reference로 변경**한다.
5. Initializing이 수행된다. 원래는 static initialization이 실행되지만, 예제 코드에선 static 필드를 포함하고 있지 않다.

이렇게 class `load`을 마쳤다. 다음은 `main()`을 호출할 단계다.

#### `main()` Method 호출 (1)
이제 `main()` method를 호출하여, 실질적으로 프로그램을 실행(excute)한다. 이는 Excution Engine에 의해 수행된다. 프로그램이 실행되려면 프로그램 흐름의 최소 단위인 thread가 있어야 한다. 따라서, **JVM은 `main()` method 호출을 위한 main thread를 생성**한다. 여기서 Excution Engine과 `per-Thread` 단위에 속하는 데이터 영역들에 대해 정확히 알아본 뒤, 다시 흐름을 진행하도록 하자.

#### 실행 엔진(Execution Engine)
용어에서 알 수 있듯이 실질적으로 **Bytecode를 명령어 단위로 읽어 실행**한다. **Bytecode의 명령어는 1byte의 OpCode와 추가 Operand**로 이루어져 있다. 앞서 언급하였듯이 Bytecode는 Native Code가 아니기 때문에 Excution Engine은 Bytecode를 기계가 실행할 수 있는 형태로 변경하며, 그 방법은 다양하다. 대표적으로 실행 방식은 다음과 같다.

* 인터프리터(Interpreter) : Bytecode 명령어를 **한 줄씩 읽어서 해석(Interpret)하고 바로 실행**한다. 한 줄씩 해석하기 때문에 한 줄에 대한 해석은 빠른 대신 전체 결과의 실행은 느리다. **Execution Engine은 기본적으로 Interpreter 방식으로 동작한다.** 초기 JVM 역시 Interpreter 방식으로만 구현되었으나, **실행 효율을 고려하여 JIT Compiler 방식을 추가**했다.
* JIT(Just-In-Time) Compiler : Interpreter 방식으로 실행하다가 별도의 알고리즘([JIT/HotSpot VM 참고](https://www.infoq.com/articles/OpenJDK-HotSpot-What-the-JIT/))에 따라 **Bytecode를 Compile하여 Native Code로 변경하고, Native Code로 직접 실행하는 방식**이다. Native Code를 실행하는 방식이 Interpreter 방식보다 빠르고, 이는 캐시에 보관하기 때문에 한 번 Compile된 Code는 매우 빠르게 실행된다. JIT Compiler를 적절히 추가하여 Interpreter 방식의 비효율성을 줄였다.

#### JVM Stack
JVM Stack은 Frame이라는 구조체를 저장하는 Stack이다. JVM은 JVM Stack에 대해 `push` 혹은 `pop` 동작만 수행한다.

**Frame은 JVM Stack에 쌓이는 정보의 단위이며, 이는 데이터나 중간 결과의 저장, 값 반환, 동적 Link등에 사용**된다. Method가 호출될 때마다 하나의 스택 프레임이 생성되고 `push`되고, method 종료 시 `pop` 된다. 이는 Local Variable Array, Operand Stack, 현재 수행 중인 method가 속한 class의 Runtime Constant Pool에 대한 Reference로 구성된다. 그 구조는 다음과 같다.
![](https://images.velog.io/images/harang/post/7ec49b9f-88a6-42ce-ab45-39fa3112e9fe/JVM%20Stack.png)

* 지역 변수 배열(Local Variable Array)
  Method가 호출될 때 그 Method의 파라미터 값은 Local Variable Array를 통해 넘겨진다(넘겨받는다).
    * Instance Method일 경우, **`this` reference가 0번 index에 저장**되고, 1번 index부터 파리미터들이 차례대로 저장된다.
    * Class Method일 경우 0번 index부터 파라미터들이 차례대로 저장된다.
  Parameter가 차례대로 저장되고 난 이후에는, **method의 지역 변수들이 저장**된다.
* Operand Stack
  각 method는 Operand Stack과 Local Variable Array 사이에서 데이터를 교환하고, 다른 Method의 반환 값을 `push`하거나 `pop`한다. **대략적으로 데이터를 가져오고 넘겨주는 거의 모든 과정이 Operand Stack을 통해 이루어진다고 보면 된다.**
* Reference to Runtime Constant Pool
  이는 용어 그대로 자신이 속한 Frame에 대응되는 Method가 속한 Class의 Runtime Constant Pool에 대한 참조를 뜻한다.

이때, Local Variable Array의 Length와 Operand Stack의 최대 깊이는 Compile Time에 결정된다. 따라서, 이는 Byte Code에서 확인할 수 있다. 아래는 예제 코드의 `main()` method의 Byte Code 중 일부이다. `stack`은 Operand Stack의 최대 깊이, `locals`는 Local Variable Array의 Length이다.
```
 Code:
      stack=1, locals=1, args_size=1
```

#### PC Register
PC Register에는 현재 실행 중인 Method가
* Native Method가 아닌 경우, 현재 실행 중인 JVM command의 위치가 저장되고,
* Native Method일 경우, PC Register에 저장되는 값은 정의되지 않는다.

#### Native Method Stack
이는 Java가 아닌 다른 언어로 작성된 Native Method를 지원하기 위해 사용되는 스택이다.

#### `main()` Method 호출 (2)
다시 `main()` Method를 호출하는 시점으로 돌아와, 예제를 살펴보자. 사실 `main()` Method를 호출한 이후에는 Execution Engine이 Byte Code를 한 줄씩 실행하는 과정만 남았다. 물론 상황에 따라 Class Loader, Runtime Data Areas와 상호작용하며 실행된다. `main()` Method의 Bytecode는 아래와 같다.

```java
public static void main(java.lang.String[]);
  descriptor: ([Ljava/lang/String;)V
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    stack=2, locals=2, args_size=1
       0: new           #7                  // class staycozyboy/java/jvm/Aloha
       3: dup
       4: invokespecial #9                  // Method "<init>":()V
       7: astore_1
       8: getstatic     #10                 // Field java/lang/System.out:Ljava/io/PrintStream;
      11: aload_1
      12: invokevirtual #16                 // Method aloooha:()Ljava/lang/String;
      15: invokevirtual #20                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      18: return
    LineNumberTable:
      line 5: 0
      line 6: 8
      line 7: 18
```

사실 Bytecode를 한 줄씩 읽어가며 세부적인 원리를 설명하려 했으나, 실행 과정의 핵심 흐름은 이전까지의 내용만으로도 충분히 자세히 정리한 것 같아 글을 마무리 하겠다. ([Opcode 참고](https://javaalmanac.io/bytecode/) + 이후의 과정을 Byte Code를 한 줄씩 읽어가며 세부적인 원리를 공부하고 싶다면 [참고](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/))

## 마무리
비록 하나의 (방대한) Topic을 다뤘다지만, 피상적이고 큰 내용부터, 깊고 세부적인 내용까지 한번에 다루고 싶은 욕심에 응집성이나 통일성을 고려하지 못한 글이 되었다. 다음부터는 글의 목적에 따라 핵심 내용만 Scoping해서 글로 작성하고, 중요도가 낮은 부분은 링크로 처리해야겠다..

## References
* [JDK docs](https://blogs.oracle.com/java/jdk-8-early-access-developer-documentation-updates)
* [javaalmanac.io/Java Bytecode](https://javaalmanac.io/bytecode/)
* [Java 면접시 빈번한 질문(옛날)](https://ryumso86.tistory.com/28)
* [JVM 동작원리](https://steady-snail.tistory.com/67)
* [Java 실행 과정](https://dev-ahn.tistory.com/121)
* [Java Compile to Run](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/)
* [Java 실행 원리](https://gbsb.tistory.com/2)
* [Java Memory Model / 성능개선을 위한 GC의 활용]
(https://stophyun.tistory.com/37)
* [JVM Internal](https://d2.naver.com/helloworld/1230)
* 윤성우의 열혈 Java 프로그래밍(윤성우, 오렌지 미디어, 2017)
* Java Performance Fundamental(김한도, 엑셈, 2009)