# 자바 컬렉션 프레임웍(Java Collection Framework)
데이터 군을 저장하는 클래스들을 표준화한 설계를 뜻한다. 다수의 데이터를 다루는 데 필요한 다양하고 풍부한 클래스들을 제공하기 때문에 프로그래머의 짐을 상당히 덜어 주고 있다. 또한, 사용법도 익히기 편하고 재사용성이 높은 코드를 작성할 수 있다.

## 컬렉션 프레임웍의 핵심 인터페이스
* List : 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.   
배열과 비슷하며, 배열은 크기를 제한하지만 List는 크기를 제한하지 않아 편리하다. List에는 대표적으로 ArrayList, Vector, LinkedList 클래스가 있다.

* Set : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다.(수학에서의 집합)    
대표적으로 HashSet, TreeSet이 있다.

* Map : 키(key)와 값(value)의 쌍(pair)으로 이루어진 데이터의 집합
순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.     
대표적으로 HashMap, TreeMap이 있다.

## set과 List의 차이점
가장 큰 차이점은 List는 순서가 있어서 데이터의 중복이 가능하지만 Set은 데이터의 중복이 불가능하다. 이 외에도 사용하는 메서드에서도 차이점이 있다.
기본적으로 Collection인터페이스에 있는 메서드들을 Set과 List는 가지고 있지만 List는 특수하게 더 가지고 있는 메서드들이 존재한다.
예를들어

    object get/set (int index)
    void add(int index, Object element)

등이 있다. get메서드는 인덱스에 해당하는 값을 반환하는것인데, set은 순서가 존재하지 않기 때문에 이 메서드가 필요가 없다. add도 마찬가지다.

## List
List는 아까 말했듯이, 배열과 비슷한 개념이다.
제네릭을 활용하여 List< E >는 원하는 데이터 타입의 List를 만들 수 있다.     
* ArrayList : 저장 용량을 초과한 객체들이 들어오면 자동적으로 저장용량이 늘어나는 구조
* LinkedList : 인접 참조를 링크해서 체인처럼 관리, 특정 인덱스의 객체를 제거하면 앞뒤 링크만 변경됨. 즉, **삽입/삭제가 빈번히 있을 때 쓰는 것이 효율적**
* Vector : ArrayList와 같은 구조를 갖고 있으나 동기화(Syncronized)된 메서드로 구성되어 있어 **멀티 쓰레딩 구조에 안정적**

List< E >의 예로 ArrayList< E >에 대해서 알아보자.

    String[] ar = new String[2];
    ar[0] = "one";
    ar[1] = "two";
    ar[2] = "three"; // 오류 발생

일반 String타입의 배열이다 이 배열의 경우 초기화 시에 배열의 길이를 정해줌으로써 배열의 길이보다 더 많은 데이터를 못 담는다.

    ArrayList<String> ar = new ArrayList<String>();
    ar.add("one");
    ar.add("two");
    ar.add("three");
    
위 코드 처럼 ArrayList< E >를 사용함으로써 배열의 길이에 제한되지 않게 된다.

## Set
Set은 수학에서의 집합과 비슷한 개념이다.
* HashSet : 순서가 필요없는 데이터를 HashTable에 저장하며, Set 중 가장 높은 성능을 보인다.
* TreeSet : 저장된 데이터의 값에 따라(a-z, 1–9) 정렬되며, Red-black tree 타입으로 값이 저장되고 HashSet보다는 성능이 느리다.

HashSet< E >의 예시이다.

    HashSet<Integer> set1 = new HashSet();
    HashSet<Integer> set2 = new HashSet();
    set1.add(1);
    set1.add(2);
    set1.add(3);
    set2.add(3);
    set2.add(4);
    set2.add(5);
    set1.addAll(set2); // 출력 : 1 2 3 4 5(합집합)
    set1.remainAll(set2); // 출력 : 3 (교집합)
    set1.removeAll(set2); // 출력 : 1 2 (차집합)

이처럼 set은 수학에서의 집합의 개념이다.

## Map
Map은 수학에서의 함수와 비슷하다. key가 정의역 value가 공역이라고 생각하면 쉽다.    
HashMap<K,V>의 예

    HashMap<String, Integer> a = new HashMap<String, Integer>();
    a.put("one",1);
    a.put("two",2);
    a.put("three",3);
    System.out.println(a.get("one"));
    System.out.println(a.get("two"));
    System.out.println(a.get("three"));

## 정렬
1. Comparator  
정렬기준을 별도로 구성하고 싶을때 사용한다. Cmparator는 compare() 메서드를 구현해야 하는데, 이 메서드는 양수를 반환하면 두 값의 순서가 바뀐다(오름차순). 

2. **Comparable**    
객체 자체에 정렬 기능을 구현하기 위해 사용한다. 그리고 Comparable은 compareTo메서드를 구현해야한다.

아래는 sort의 실제 메서드이다.

    public static <T extends Comparable<? super T>> void sort(List<T> list) {
        Object[] a = list.toArray();
        Arrays.sort(a);
        ...
    }

여기서 살펴보면 sort함수는 List타입만 정렬이 가능하다. 그리고 T는 Comparable인터페이스를 구현하고 있어야한다. 그리고 <? super T>를 보면 T또는 T의 조상 클래스타입도 가능하다. 즉, T만되는게 아니라 T의 상위 클래스까지도 Comparable할 수 있다는 소리이다.