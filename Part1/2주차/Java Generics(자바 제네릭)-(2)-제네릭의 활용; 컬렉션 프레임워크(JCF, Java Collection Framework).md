# 컬렉션 프레임워크(JCF)
*시작에 앞서, 이 글은 책, 강의, 구글링등을 통해 개인적으로 학습 및 실습한 개념을 정리한 글이며, 정확하지 않은 내용이 있을 수 있습니다. 꾸준히 공부해가며 수정하겠습니다. 또한, 오류 지적은 언제나 환영입니다.*

이 글은 JCF, Lambda, Stream등 자바의 핵심 요소를 정확히 이해하기 위해 반드시 학습해야하는 제네릭(Generics)을 2편에 나누어 다루고 있다.

1. 제네릭의 이해
2. 제네릭의 활용; 컬렉션 프레임워크

## 개요
* JCF의 개념
* JCF 살펴보기
* 제네릭의 활용; JCF 기반 알고리즘

## JCF의 개념
자료구조와 알고리즘에 대한 기초가 튼튼한 사람이라면, 컬렉션 프레임워크의 활용은 그리 어렵지 않을 것이다. 따라서, **이 글은 JCF에 대해서 상세히 다루지 않는다**(JCF를 설명하고 있는 글은 많으니, 그 중 [좋은 글](https://www.javatpoint.com/collections-in-java)을 레퍼런스에 추가해두겠다). **대신 제네릭에 대한 부족한 이해를 보충하는데 초점을 맞춰 JCF를 살펴보도록 하겠다.** 먼저, JCF의 개념에 대해 살펴보자.

[이 곳](https://www.javatpoint.com/collections-in-java)에서는 다음과 같이 정의하고 있다.

>The Collection in Java is a framework that provides **an architecture to store and manipulate the group of objects.**

즉, JCF는 데이터(objects)를 효과적으로 저장하고 다룰 수 있는(manipulate) 프레임워크다. 쉽게 말하면, **기초적인 자료구조(List, Set, Map 등)와 알고리즘(Sort, Search 등)을 제네릭 기반의 클래스와 메소드로 미리 구현해 놓은 결과물**이다. 따라서 이를 활용하면, 직접 구현하지 않고도 자료구조 혹은 알고리즘의 이점을 그대로 프로그램에 가져올 수 있다.

## JCF 살펴보기
먼저, JCF에 대해 간단하게 알아보자. 대략적인 구조는 다음과 같다.

![](https://images.velog.io/images/harang/post/31b6ace5-62f6-43b5-a49c-6cd2436e62c6/image.png)

이미지 출처: https://www.startertutorials.com/corejava/introduction-java-collections-framework.html

구글링해보면 JCF 구조에 대한 많은 이미지들이 있지만, 그 중에서 가장 적절한 수준으로 복잡한 것 같다(+ 정확하다 / 좀 부족하다 느껴지면 [참고](https://prashantgaurav1.files.wordpress.com/2013/12/java-util-collection.gif)). 아쉬운 점은 HashSet은 포함시켰는데, HashMap은 없다는 점.. 간략하게 보면, JCF의 핵심 인터페이스들 간의 상속 관계는 다음과 같다.

![](https://images.velog.io/images/harang/post/59a2c8f0-295b-46f5-9757-3563ca9917cf/JCF.png)

앞서 말했듯이, JCF에 대해서 깊게 다루진 않을 것이다. JCF의 기본 자료구조 클래스들을 쫙 나열하며 정리할 것이니, 참고하시면 되겠다.(자세한 내용은 [참고](https://www.javatpoint.com/collections-in-java)) 자료구조를 구현한 클래스인 만큼 어떻게 **참조, 추가/삭제, 순회**가 이루어지는 지 중점적으로 공부하면 되겠다.

### `List<E>`
* `ArrayList<E>`
  * 배열 기반 자료구조.
  * 원소의 추가/삭제 연산이 느리다. O(N).
  * 원소의 참조가 빠르다. O(1).
* `LinkedList<E>`
  * 연결 리스트 기반 자료구조.
  * 원소의 추가/삭제 연산이 빠르다. O(1).
  * 원소의 참조가 느리다. O(1).
  * `listIterator()`를 이용해 양방향 반복자를 얻을 수 있으며, 이는 양방향 이동을 가능케 해준다.

`List<E>`를 구현하는 클래스들은 다음과 같은 공통점을 가지고 있다.
* 데이터의 저장 순서를 유지한다.
* 동일한 인스턴스의 중복 저장을 허용한다.
* `Iterable<T>`를 상속한다. 따라서, `iterator()`를 사용해 순회할 수 있다.
* 또한, enhanced for loop을 사용해 순회할 수 있다.


### `Set<E>`
* `HashSet<E>` (이에 대한 자세한 사항은 [이 글](https://www.baeldung.com/java-hashset)을 참고.)
* `TreeSet<E>`
  * Binary Tree 기반으로 구현되었다.
  * SortedSet 즉, 특정 기준에 의해 정렬되어 저장된다. 따라서, `Comparable<T>` 구현.
  * `Compartor<T>`를 이용해 정렬 기준을 유동적으로 정할 수 있다.

`Set<E>`를 구현하는 클래스들은 말그대로 집합을 자료구조화 시킨 개념이며, 다음과 같은 공통점을 가지고 있다.
* 데이터의 저장 순서가 유지되지 않는다.
* 데이터의 중복 저장을 허용하지 않는다.

이때, `Set<E>`가 데이터의 중복을 판단하는 방식을 알아보자. 간단하게 정리하면, 이는 다음과 같다. (Hash Algorithm에 대한 자세한 내용은 [이 글](https://hsp1116.tistory.com/35)을 참고.)

1. `hashCode()` 메서드를 호출해 해쉬값 비교. 이때 해쉬값이 같다면 같은 탐색 부류(bucket)에 속함을 뜻한다.
2. 해쉬값이 같다면(같은 bucket이라면), `equals()` 메서드를 호출하여 중복되는 인스턴스인지 판단한다. 

### `Queue<E>`
* `Stack<E>`
  * 이 클래스는 `Vector<E>`처럼 자바 초기에 정의된 클래스로써 지금은 호환성 유지를 위해 존재할 뿐 쓰지 않는다. 대신 Deque을 쓰면 된다.
  * LIFO 구조.
* `Queue<E>`
  * FIFO 구조.
  * 보통 `LinkedList<E>`나 `ArrayDeque<E>`을 통해 사용한다.
* `Deque<E>`
  * Double-ended Queue라는 이름 그대로 front과 back에서 pop/push가 가능한 구조이다.
  * 보통 `ArrayDeque<E>`를 통해 사용한다.

`Queue<E>`를 구현하는 클래스들은 다음과 같은 공통점을 가지고 있다.
* pop/push 연산은 모두 O(1)이다.
* peek과 size 연산 또한 O(1)이다.

### `Map<K, V>`
* `HashMap<K, V>`
  * HashSet의 Map 버전이라고 보면 된다.
* `TreeMap<K, V>`
  * SortedMap을 구현한다. 이는 `TreeSet<E>`처럼 특정 기준에 따라 정렬되어 저장된다. 마찬가지로 `Comparator<T>`를 이용해 정렬 기준을 유동적으로 정할 수 있다.

`Map<K, V>`를 구현하는 클래스들은 다음과 같은 공통점을 가지고 있다.
* Key와 Value의 쌍으로 이루어진 데이터의 집합이다.
* 데이터의 저장 순서가 유지되지 않는다.
* Key값의 중복을 허용하지 않는다. 다만, Value의 경우는 중복을 허용한다.
* `keySet()` 메서드를 호출하여 key값의 set(`Set<E>`를 구현하는 컬렉션 인스턴스)을 얻을 수 있다. 이를 통해 순회를 수행할 수 있다.

## 제네릭의 활용; JCF 기반 알고리즘
다음 제네릭 메서드들의 제네릭(+와일드카드) 선언이 갖는 의미에 대해 생각해보자. 참고로 이들은 실제로 `java.util.Collections`에 정의된 메서드들이다. ([참고](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html))
* `public static <T extends Comparable<? super T>> void sort(List<T> list)`
* `public static <T> void sort(List <T> list, Comparator<? super T> c)`
* `public static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`
* `public static <T> int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)`
* `public static <T> void copy(List<? super T> dest, List<? extends T> src)`

얼핏봐도 꽤 복잡하다. 이 정도의 선언을 이해하고 있다면, 휼륭하다. 제네릭을 매우 잘 이해하고 있다. 만약 잘 모르겠더라도 괜찮다. 이 글을 읽으며 같이 공부해보자. 글을 읽기 전에, 제네릭 1편(제네릭의 이해)를 읽고 오는 것을 추천한다.

### 정렬
지금 살펴볼 `java.util.Collections`에 구현되어 있는 sort 메서드의 경우 merge sort를 구현했다. 따라서, O(NlogN)에 정렬할 수 있다. **sort 메서드의 경우 정렬 기준(비교 기준)이 `Comparable<T>` 기반이냐, `Comparator<T>` 기반이냐에 따라 나누어 두 가지**로 정의되어있다. 먼저, `Comparable<T>` 기반의 정의를 살펴보자.

`public static <T extends Comparable<? super T>> void sort(List<T> list)`

가장 어렵게 다가올 부분은 `extends Comparable<? super T>`일 것이라 생각한다. 이 부분을 제외하고 다시 살펴보자

`public static <T extends Comparable<T>> void sort(List<T> list)`

비교적 쉽게 이해된다. 이를 해석해보면 다음과 같은 내용을 알 수 있다.
* 메소드 호출 시점에 결정된 T에 대해 **`List<T>`의 모든 인스턴스는 인자로 전달될 수 있다.**
* 이때, **T는 `Comparable<T>` 인터페이스를 구현한 클래스**이어야 한다.

위의 내용에 `extends Comparable<? super T>`에 대한 설명을 추가해 완전히 해석해보자. **이는 `Comparable<T>`를 구현하는 특정한 클래스 T를 상속하는 클래스로 하여금 T의 정렬 기준(T가 오버라이딩한 compareTo 메서드)을 그대로 사용하여 정렬할 수 있게 하기 위함이다.** 예를 들어, `Comparable<Box>`를 구현하는 Box 클래스와 Box 클래스를 상속하는 PaperBox 클래스가 있다고 가정하자. Box와 PaperBox는 다음과 같다.

```java
class Box implements Comparable<Box> {
	...
    @Override
    public int compareTo(Box box) {...} // compareTo 메서드가 오버라이드 되어있다.
    ...
}

class PaperBox extends Box {
	... // compareTo 메서드를 오버라이드 하고 있지 않다.
}
```
주석을 읽어보면 PaperBox의 경우, compareTo 메서드를 호출하면 Box의 compareTo 메서드가 호출됨을 알 수 있다. PaperBox 인스턴스를 인자로 하여 sort 메서드를 호출하는 상황을 생각해보자. `extends Comparable<? super T>`가 없다면, PaperBox는 별도로 `Comparable<PaperBox>`를 구현해야할 것이다. 하지만, 정렬 기준이 상위 클래스와 같은 경우, 이는 매우 비효율적이다. 따라서, sort 메서드는 다음과 같이 정의해야한다.

`public static <T extends Comparable<? super T>> void sort(List<T> list)`

위의 경우, PaperBox가 별도로 `Comparable<PaperBox>`를 구현할 필요없이, Box의 compareTo 메서드를 기준으로 정렬될 수 있다.

다음으로 `Comparator<T>`기반의 sort 메서드를 살펴보자. 앞서 다뤘던 내용이기에 비교적 쉽게 이해할 수 있을 것이다.

`public static <T> void sort(List <T> list, Comparator<? super T> c)`

이를 해석해보면 다음과 같은 내용을 알 수 있다.
* 메소드 호출 시점에 결정된 T에 대해 **`List<T>`의 모든 인스턴스는 첫 번째 인자(list)로 전달될 수 있다.**
* 두 번째 인자(c)의 경우 **특정한 클래스 T를 상속하는 클래스도 `Compartor<T>`를 구현하는 compartor 인스턴스의 정렬 기준을 사용해 정렬할 수 있다.**(표현이 이상한지 많이 복잡한 문장이 되어버리는데, 찬찬히 의미를 따져보면 어렵지 않는 내용이다.)

### 탐색
탐색의 경우 메서드의 이름 그대로 이진 탐색 알고리즘으로 구현되어있다. 따라서, 정렬된 컬렉션 인스턴스에 대해 O(logN)만에 특정 원소를 탐색할 수 있다. 정렬과 마찬가지로 **`Comparable<T>` 기반이냐, `Comparator<T>` 기반이냐에 따라 나누어 두 가지**로 정의되어있다. 앞서 정렬에서 다뤘던 내용의 반복이므로 간단하게 설명만 하겠다. 먼저, `Comparable<T>` 기반의 `binarySearch()` 메서드.

`public static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`

* 첫 번째 인자(list)를 먼저 살펴보자.**`List<? extends T>`는 list가 참조하는 인스턴스를 대상으로 get만 호출할 수 있음을 의미**한다. (이는 탐색 메서드 안에서 list의 원소를 참조하여 key와 비교만 하면 되기 때문에 바람직한 구현임.)
* `extends Comparable<? super T>`는 **`Comparable<T>`를 구현하는 특정한 클래스 T를 상속하는 클래스로 하여금 T의 정렬 기준(T가 오버라이딩한 compareTo 메서드)을 그대로 사용하여 정렬할 수 있다는 의미**이다.
* T key는 자명하기 때문에 생략하겠다.

다음은 `Comparator<T>` 기반의 `binarySearch()` 메서드.

`public static <T> int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)`

* 첫 번째 인자(list)를 먼저 살펴보자.**`List<? extends T>`는 list가 참조하는 인스턴스를 대상으로 get만 호출할 수 있음을 의미**한다.
* T key는 자명하기 때문에 생략하겠다.
* 세 번째 인자(c)의 경우 **특정한 클래스 T를 상속하는 클래스도 `Compartor<T>`를 구현하는 compartor의 정렬 기준을 사용해 정렬할 수 있음을 의미**한다.

### 복사
앞서 살펴본 정렬과 탐색 메서드에 비해선 비교적 쉽다. 리스트 인스턴스(src)의 값을 복사하여 다른 인스턴스(dest)에 저장하는 메서드이다. 이미 생성된 인스턴스의 내용을 통째로 바꾸려는 경우 유용하게 사용할 수 있다. 이때 dest의 length는 src보다 크거나 최소한 같아야 한다. 이를 지키지 않을 시 예외를 발생시킨다.

**최대한 컴파일 과정 중에 오류를 발생**시키도록 프로그래밍을 해야한다는 사실은 자명하다. 이 관점에서, **src의 경우, get만 허용하고, dest의 경우 set만 허용**하도록 구현하는 것이 바람직하다. 따라서 메서드는 다음과 같이 정의되어있다.

`public static <T> void copy(List<? super T> dest, List<? extends T> src)`

* `List<? super T> dest`의 경우 T 타입의 인스턴스를 인자로 받아 set만 허용하겠다는 의미이다.
* `List<? extends T> src`의 경우 T 타입의 인스턴스를 인자로 받아 get만 허용하겠다는 의미이다.

## Reference
* [Collections in Java](https://www.javatpoint.com/collections-in-java)
* [Collection Framework](https://pridiot.tistory.com/63)
* [Java Collection Framework](https://www.ict.social/java/collections-and-streams-in-java/java-collections-framework)
* [Java Specification Docs / Collections](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html)
* 윤성우의 열혈 Java 프로그래밍(윤성우, 오렌지 미디어, 2017)