# 컬렉션 프레임워크(Collection Framework)
애플리케이션을 개발하다 보면 다수의 객체를 저장해두고 필요할 때마다 꺼내서 사용하는 경우가 많다. 어떻게 객체를 효율적으로 추가, 검색, 삭제할 지 고민해야하는데 가장 간단한 방법은 배열을 이용하는 것이다. 배열은 쉽게 생성하고 사용할 수 있지만, 저장할 수 있는 객체의 수가 배열을 생성할 때 정해지기에 불특정 다수의 객체를 저장하기에는 문제가 있다. 배열의 또 다른 문제점은 객체를 삭제했을 때 해당 인덱스가 비게 되어 낱알이 듬성듬성 빠진 옥수수가 될 수 있다.  
자바는 배열의 이러한 문제점을 해결하고, 널리 알려져 있는 자료구조(Data Structure)을 바탕으로 객체들을 객체들을 효율적으로 추가, 삭제, 검색할 수 있도록 java.util 패키지에 컬렉션과 관련된 인터페이스와 클래스들을 포함시켜 놓았다. 이들을 총칭해서 컬렉션 프레임워크(Collection Framework)라고 부른다. 자바의 컬렉션(Collection)은 객체를 수집해서 저장하는 역할을 한다. 프레임 워크란 사용 방법을 미리 정해놓은 라이브러리를 말한다. 
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbdy438%2FbtqEjPZKIY0%2Fe5Wm8ZJmdRNza4tKBzaK6k%2Fimg.png)

## List 컬렉션
List 컬렉션은 객체를 일렬로 늘어놓은 구조를 가지고 있다. 객체를 인덱스로 관리하기 때문에 객체를 저장하면 자동 인덱스가 부여되고 인덱스로 객체를 검색, 삭제할 수 있는 기능을 제공한다. List 컬렉션은 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조한다. 동일한 객체를 중복 저장할 수 있는데, 이 경우 동일한 번지가 참조된다. null도 저장 가능한데, 이 경우 해당 인덱스는 객체를 참조하지 않는다.(번지 : 주소값)
![](https://t1.daumcdn.net/cfile/tistory/990FC93F5A5FEB1527)
List 컬렉션에서 공통적으로 사용 가능한 List 인터페이스의 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/2630DC3755F37FFD30)

위의 메서드들은 List 인터페이스에서 추상메서드로 선언해놓고 List인터페이스를 구현한 클래스에서 구현한 방식이다. 그리고 메서드에서 알 수 있듯이 List 인터페이스의 메서드들 중 제네릭 메서드들이 있다. 고로 List 인터페이스가 제네릭 인터페이스임을 알 수 있다.

### ArrayList 클래스
일반 배열과 ArrayList는 인덱스로 객체를 관리한다는 점은 비슷하지만, 배열은 생성할 때 크기가 고정되고 사용 중에 크기를 변경할 수 없지만 ArrayList는 저장 용량을 초과한 객체들이 들어오면 자동적으로 저장 용량이 늘어난다.

	List<String> list = new ArrayList<String>(30);	//String 객체 30개를 저장할 수 있는 용량을 가짐.

ArrayList 클래스에서 제네릭을 사용하는 이유?
> 제네릭을 사용하기 전에는, 객체를 저장할 때 Object타입으로 변환되어 저장되었다. `List list = new ArrayList();` 모든 종류의 객체를 저장할 수 있다는 장점은 있지만 저장할 때 Object로 변환하고 찾아올 때 원래타입으로 변화해야하므로 실행성능에 좋지 않은 영향을 끼쳤다. 
> 그래서 자바5부터 제네릭을 도입하여 ArrayList 객체를 생성할 때 타입 파라미터로 저장할 객체의 타입을 지정함으로써 불필요한 타입 변환을 하지 않도록 했다.

##### Arrays.asList(T... a);
ArrayList를 생성하고 런타임 시 필요에 의해 객체들을 추가하는 것이 일반적이지만, 고정된 객체들로 구성된 List를 생성할 때도 있다. 이런 경우에는 `Arrays.asList(T... a);`를 사용한다.


	import java.util.Arrays;
	import java.util.List;

	public class ArrayAsListExample {
		public static void main(String[] args) {
			List<String> list1 = Arrays.asList("홍길동", "신용권", "감자바");
			for (String name : list1) {
				System.out.println(name);
			}
		
			List<Integer> list2 = Arrays.asList(1, 2, 3);
			for (int value : list2) {
				System.out.println(value);
			}
		}
	}


### Vector 클래스
Vector는 ArrayList와 동일한 내부구조를 가지고 있다. 다른 점은 Vector는 동기화된(synchronized) 메서드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 메서드들을 실행할 수 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행할 수 있다. 그래서 멀티 스레드 환경에서 객체를 안전하게 추가, 삭제할 수 있다. 이것을 *스레드가 안전(Thread Safe)*하다라고 말한다. (스레드는 간단히 말하면 작업의 단위다)

### LinkedList 클래스
LinkedList클래스는 List구현 클래스이므로 ArrayList와 사용방법은 같지만 내부 구조는 완전 다르다. ArrayList는 내부 배열에 객체를 저장해서 인덱스로 관리하지만, LinkedList는 인접  참조를 링크해서 체인처럼 관리한다.

![](https://t1.daumcdn.net/cfile/tistory/216C813755F3800508)

LinkedList에서 특정 인덱스의 객체를 제거하면 앞뒤 링크만 변경되고 나머지 링크는 변경되지 않는다. 특정 인덱스에 객체를 삽입할 때에도 마찬가지다. 그렇기 때문에 빈번한 객체 삭제와 삽입이 일어나는 곳에서는 ArrayList보다 LinkedList가 좋은 성능을 발휘한다.

+ *ArrayList 클래스에서 remove 메서드를 사용해서 객체를 삭제하면 바로 뒤 인덱스부터 마지막 인덱스까지 모두 앞으로 1씩 당겨진다. 마찬가지로 add메서드를 통해 삽입하면 해당 인덱스부터 마지막 인덱스까지 모두 1씩 밀려난다.*

+ 끝에서부터(순차적으로) 추가/삭제하는 경우는 ArrayList가 더 빠르지만, 중간에 추가 또는 삭제할 경우는 앞뒤 링크 정보만 변경하면 되는 LinkedList가 더 빠르다.


## Set 컬렉션
List컬렉션은 저장순서가 유지되지만, Set컬렉션은 저장 순서가 유지되지 않는다. 또한 객체를 중복해서 저장할 수 없고 하나의 null만 저장할 수 있다. 수학의 집합에 비유된다. 순서와 상관없고 중복이 허용되지 않는다.

![](https://mblogthumb-phinf.pstatic.net/20160503_67/qkrghdud0_14622359319609yxss_PNG/Set%B8%DE%BC%D2%B5%E5.png?type=w2)

Set컬렉션은 인덱스로 객체를 검색해서 가져오는 메서드가 없다. 대신, 전체 객체를 대상으로 한 번씩 반복해서 가져오는 반복자(Iterator)를 제공한다. 반복자는 Iterator 인터페이스를 구현한 객체를 말하는데, iterator() 메서드를 호출하면 얻을 수 있다.

	Set<String> set = ...;
	Iterator<String> iterator = set.iterator();

![](https://mblogthumb-phinf.pstatic.net/20160503_288/qkrghdud0_1462236443843EUYYW_PNG/Iterator.png?type=w2)

### Iterator
Iterator 인터페이스는 컬렉션 프레임워크에서 저장된 요소를 읽어오는 방법을 표준화하기 위한 역할이다. 또, Collection Framework의 한 멤버이다.
Java에서 제공하는 컬렉션(Collection)객체는 보관하고 있는 자료들을 순차적으로 접근하면서 처리할 때 사용하는 Iterator 형식을 제공하고 있다. 주로 List, Set 컬렉션에서 쓰인다.
Iterable 인터페이스는 iterator에 의해 반환되는 값을 타입 파라미터로 가진다.

Iterator method에는 hasNext(), next(), remove()가 있다. 
hasNext() : 읽어올 요소가 남아있는지 확인하는 메서드. 요소 있으면 true, 없으면 false
next() : 다음 데이터를 반환한다.
remove() : next()로 읽어온 요소를 삭제한다.


### HashSet 
HashSet은 객체들을 순서 없이 저장하고 동일한 객체는 중복 저장하지 않는다. HashSet이 판단하는 동일한 객체란 꼭 같은 인스턴스를 뜻하지는 않는다. HashSet은 객체를 저장하기 전에 먼저 객체의 hashCode() 메서드를 호출해서 해시코드를 얻어낸다. 그리고 이미 저장되어 있는 객체들의 해시코드와 비교한다. 만약 equals() 메서드로 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장하지 않는다.

![](https://image.slidesharecdn.com/collectionframework-190628181221/95/collection-framework-30-638.jpg?cb=1561745613)


## Map 컬렉션
Map컬렉션은 키(key)와 값(value)로 구성된 Entry(Map인터페이스의 내부 인터페이스) 객체를 저장하는 구조를 가지고 있다. 여기서 키와 값은 모두 객체다. 키는 중복 저장할 수 없지만, 값은 중복 저장될 수 있다.

![](https://image.slidesharecdn.com/collectionframework-190628181221/95/collection-framework-39-638.jpg?cb=1561745613)

Map컬렉션에는 **HashMap, Hashtable, LinkedHashMap, Properties, TreeMap**등이 있다.

키를 알고 있다면 get()메서드로 간단하게 객체를 찾아오면 되지만, 저장된 전체 객체를 대상으로 하나씩 얻고 싶을 경우에는 두 가지 방법을 사용할 수 있다. 첫 번째는 keySet() 메서드로 모든 키를 Set컬렉션으로 얻은 다음, 반복자를 통해 키를 하나씩 얻고 get() 메서드를 통해 값을 얻으면 된다.

	Map<K, V> map = ~ ;
	Set<K> keySet = map.keySet();
	Iterator<K> keyIterator = keySet.iterator();
	while(keyIterator.hasNext()) {
		K key = keyIterator.next();
		V value = map.get(key);
	}

두 번째 방법은 entrySet() 메서드로 모든 Map.Entry를 Set 컬렉션으로 얻은 다음, 반복자를 통해 Map.Entry를 하나씩 얻고 getKey()와 getValue() 메서드를 이용해 키와 값을 얻으면 된다.

	Set<Map.Entry<K, V>> entrySet = map.entrySet();
	Iterator<Map.Entry<K, V>> entryIterator = entrySet.iterator();
	while(entryIterator.hasNext()){
		K key = entry.getKey();
		V value = entry.getValue(key);
	}


### HashMap
HashMap은 Map인터페이스를 구현한 대표적인 Map 컬렉션이다. HashMap의 키로 사용될 객체는 hashCode()와 equals() 메서드를 재정의해서 동등 객체가 될 조건을 정해야 한다. 동등 객체, 즉 동일한 키가 될 조건은 hashCode()의 리턴값이 같아야하고, equals() 리턴값이 true여야 한다.

### Hashtable
Hashtable과 HashMap은 동일한 내부 구조를 가지고 있다. Hashtable도 키로 사용할 객체는 hashCode()와 equals() 메서드를 재정의해서 동등 객체가 될 조건을 정해야한다. HashMap과의 차이점은 동기화된(synchronized) 메서드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 메서드들을 실행할 수는 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행할 수 있다. 

### Properties
Properties는 Hashtable의 하위클래스이기 때문에 Hashtable의 모든 특징을 그대로 가지고 있다. 차이점은 Hashtable은 키와 값을 다양한 타입으로 지정이 가능한데 비해 Properties는 키와 값을 String 타입으로 제한한 컬렉션이다. 

+ Properties는 애플리케이션의 옵션 정보, 데이터베이스 연결정보 그리고 국제화(다국어) 정보가 저장된 프로퍼티(~.properties) 파일을 읽을 때 주로 사용한다.

+ 프로퍼티 파일은 키와 값이 =기호로 연결되어 있는 텍스트 파일로 ISO 8859-1 문자 셋으로 저장된다. 이 문자셋으로 직접 표현할 수 없는 한글은 유니코드(Unicode)로 변환되어 저장된다.


		driver = oracle.jdbc.OracleDriver		/*database.properties, 키와 값으로 구성된 프로퍼티*/
		url = jdbc:oracle:thin:@localhost:1521:orcl
		username = scott
		password = tiger

프로퍼티 파일은 일반적으로 클래스파일과 함께 저장된다. 클래스 파일을 기준으로 상대 경로를 이용해서 프로퍼티 파일의 경로를 얻으려면 Class의 getResoure() 메서드를 이용하면 된다. getResoure()는 주어진 파일의 상대 경로를 URL 객체로 리턴하는데, URL의 getPath()는 파일의 절대 경로를 리턴한다.
다음은 database.properties 파일로부터 값을 읽어 출력하는 예제다.

import java.io.FileReader;
import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;
import java.util.Properties;

	public class PropertiesExample {
		public static void main(String[] args) throws Exception {
			Properties properties = new Properties();
			String path = PropertiesExample.class.getResource("database.properties").getPath();		//getResoure는  Class 메서드, getPath는 URL 메서드
			path = URLDecoder.decode(path, "utf-8");		//경로에 한글 있을 경우 한글 복원하는 코드
			properties.load(new FileReader(path));
			
			/*getProperty는 Properties의 메서드, Properties도 Map컬렉션이므로 get메서드로 값을 얻을 수 있지만,
			 get메서드는 Object로 리턴하기 때문에 String으로 형변환해야한다. 따라서 일반적으로 getResoure메서드를 사용*/
			String driver = properties.getProperty("driver");	
			String url = properties.getProperty("url");
			String username = properties.getProperty("usernaem");
			String password = properties.getProperty("password");
		
			System.out.println("driver : " + driver);
			System.out.println("url : " + url);
			System.out.println("username : " + username);
			System.out.println("password : " + password);
		}
	}
실행값은 아래와 같이 나온다. key값에 따른 value 값이 나온 것을 볼 수 있다.

	driver : oracle.jdbc.OracleDriver
	url : jdbc:oracle:thin:@localhost:1521:orcl
	username : null
	password : tiger

## 검색 기능을 강화시킨 컬렉션
컬렉션 프레임워크는 검색기능을 강화시킨 TreeSet과 TreeMap을 제공하고 있다. 이 컬렉션들은 이진 트리(binary tree)를 이용해서 계층적 구조(Tree 구조)를 가지면서 객체를 저장한다. 

### 이진 트리 구조
이진 트리(binary tree)는 여러 개의 노드가 트리 형태로 연결된 구조로, 루트 노드라고 불리는 하나의 노드에서부터 시작해서 각 노드에 최대 2개의 노드를 연결할 수 있는 구조를 가지고 있다. 위 아래로 연결된 두 노드를 부모-자식 관계에 있다고 한다.

![](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F2116B34557D7E5A22CB5D7)

+ 만약 숫자가 아닌 문자를 저장할 경우에는 문자의 유니코드 값으로 비교한다.   
+ 왼쪽 마지막 노드에서 오른쪽 마지막 노드까지 [왼쪽 노드 -> 부모 노드 -> 오른쪽 노드] 순으로 값을 읽으면 오름차순으로 정렬된 값을 얻을 수 있고, 
+ 반대로 오른쪽 마지막 노드에서 왼쪽 마지막 노드까지 [오른쪽 노드 -> 부모 노드 -> 왼쪽 노드] 순으로 값을 읽으면 내림차순으로 정렬된 값을 얻을 수 있다.
+ 이진 트리가 범위 검색을 쉽게 할 수 있는 이유는 그림에서 보는 것과 같이 값들이 정렬되어 그룹핑이 쉽기 때문이다.(그룹핑 : 서브트리처럼 묶는 것을 말한다.) 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F2260B638569209CD046AC9)


### TreeSet
TreeSet은 이진 트리를 기반으로한 Set컬렉션이다. 하나의 노드는 노드값인 value와 왼쪽과 오른쪽 자식 노드를 참조하기 위한 두 개의 변수로 구성된다. TreeSet에 객체를 저장하면 자동으로 정렬되는데 부모값과 비교해서 낮은 것은 왼쪽 자식노드에, 높은 것은 오른쪽 노드에 저장한다. 

![](https://t1.daumcdn.net/cfile/tistory/265EE53555F672291E)

아래는 TreeSet이 가지고 있는 정렬과 관련된 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/23711A3555F6722C0E)

Iterator의 사용방법은 위에서 알아보았다. NavigableSet은 TreeSet과 마찬가지로 위에서 보여준 메서드를 제공하고 정렬순서를 바꾸는 descendingSet() 메서드도 제공한다. 만약 내림차순으로 정렬하고 싶다면 한번만 호출하고 오름차순으로 정렬하고 싶다면 아래와 같이 두 번 호출하면된다.  

	Navigable<E> descendingSet = treeSet.descendingSet();
	Navigable<E> ascendingSet = descendingSet.descendingSet();

다음은 TreeSet이 가지고 있는 범위 검색과 관련된 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/225E7B4055FCF92A10)

위에서 말하는 낮은 객체 높은 객체란 숫자로는 말 그대로 높고 낮음을 의미하고 문자로는 유니코드 값을 의미한다.

### TreeMap
TreeMap은 이진트리를 기반으로 한 Map 컬렉션이다. TreeSet과의 차이점은 키와 값이 저장된 Map.Entry를 저장한다는 점이다. TreeMap에 객체를 저장하면 자동으로 정렬되는데, 기본적으로 부모 키 값과 비교해서 낮은 것은 왼쪽 자식 노드에, 키 값이 높은 것은 오른쪽 자식 노드에 Map.Entry 객체를 저장한다.

![](http://www.thejavageek.com/wp-content/uploads/2016/06/FifthObjectInserted.png)

TreeMap을 생성하기 위해서는 키로 저장할 객체 타입과 값으로 저장할 객체 타입을 타입 파라미터로 주고 기본 생성자를 호출하면 된다.

	TreeMap<K, V> treeMap = new TreeMap<K, V>();
Map 인터페이스 타입 변수에 대입해도 되지만 TreeMap 클래스 타입으로 대입한 이유는 특정 객체를 찾거나 범위 검색과 관련된 메서드를 사용하기 위한 것이다.
다음은 TreeMap이 가지고 있는 검색관련 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/253D824055FCF9252B)

다음은 TreeMap이 가지고 있는 정렬과 관련된 메서드다.

	NavigableMap<K, V> descendingMap = treeMap.descendingMap();		//내림차순
	NavigableMap<K, V> descendingMap = descendingMap.descendingMap();		//오름차순

다음은 TreeMap이 가지고 있는 범위 검색과 관련된 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/225E7B4055FCF92A10)


### Comparable과 Comparator
TreeSet의 객체와 TreeMap의 키는 저장과 동시에 자동 오름차순으로 정렬되는데, 숫자(Integer, Double) 타입일 경우에는 값으로 정렬하고, 문자열(String)의 경우에는 유니코드로 정렬한다. TreeSet과 TreeMap은 정렬을 위해 `java.lang.Comparable`을 구현한 객체를 요구하는데, Integer, Double, String은 모두 Comparable 인터페이스를 구현하고 있다. 사용자 정의 클래스도 Comparable을 구현한다면 자동 정렬이 가능하다. Comparable에는 compareTo()라는 메서드가 정의되어 있기 때문에 사용자 정의 클래스에서는 이 메서드를 오버라이딩하여 다음과 같이 리턴 값을 만들어 내야 한다. 

![](https://t1.daumcdn.net/cfile/tistory/246EA34055FCF92C05)

	public class Member implements Comparable<Member>{
		private int memberId;
		private String memberName;
	
		@Override
		public int compareTo(Member member) {
			return (this.memberId - member.memberId);		//오름차순
		}
	}

만약, TreeSet 객체와 TreeMap의 키가 Comparable을 구현하고 있지 않을 경우에는 저장하는 순간 ClassCastException이 발생한다. 그렇다면 Comparable 비구현 객체를 정렬하는 방법은 없을까?

	TreeSet<E> treeSet = new TreeSet<E>(new AscendingComparator());		//오름차순 정렬자
	TreeSet<E> treeSet = new TreeSet<E>(new DescendingComparator());	//내림차순 정렬자

TreeSet 또는 TreeMap 생성자의 매개값으로 정렬자(Comparator)을 제공하면 Comparable 비구현 객체도 정렬시킬 수 있다.

정렬자는 Comparator 인터페이스를 구현한 객체를 말하는데, Comparator에는 다음과 같은 메서드가 정의되어 있다.

![](https://t1.daumcdn.net/cfile/tistory/2350E83F55FCF93124)

	import java.util.Comparator;
	public class DescendingComparator implements Comparator<Fruit> {			/*DescendingComparator.java*/
		@Override
		public int compare(Fruit o1, Fruit o2) {
			if (o1.price < o2.price) {
				return 1;									//가격이 적을 경우 뒤에 오게함.
			} else if (o1.price == o2.price) {
				return 0;									//가격이 높을 경우 앞에 오게함.
			} else {
				return -1;
			}
		}
	}

	public class Fruit {					/*Fruit.java*/
		public String name;
		public int price;
	
		public Fruit(String name, int price) {
			this.name = name;
			this.price = price;
		}
	}

(Fruit 클래스는 Comparable을 구현하지 않은 클래스다.)


	import java.util.TreeSet;
	import java.util.Iterator;

	public class ComparatorExample {
		public static void main(String[] args) {
			TreeSet<Fruit> treeSet = new TreeSet<Fruit>(new DescendingComparator());		//내림차순 정렬자 제공
			treeSet.add(new Fruit("포도", 3000));
			treeSet.add(new Fruit("수박", 10000));
			treeSet.add(new Fruit("딸기", 6000));
			
			Iterator<Fruit> iterator = treeSet.iterator();
			while(iterator.hasNext()) {
				Fruit fruit = iterator.next();
				System.out.println(fruit.name + ":" + fruit.price);
			}
		}	
	}


## LIFO와 FIFO 컬렉션
컬렉션 프레임워크에는 LIFO 자료구조를 제공하는 스택(Stack) 클래스와 FIFO 자료구조를 제공하는 큐(Queue) 인터페이스가 있다. 

![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99F33B455B73E7431C)

스택을 응용한 대표적인 예는 JVM 스택 메모리 (Stack 영역)이다. 스택 메모리에 저장된 변수는 나중에 저장된 것부터 제거된다.
큐를 응용한 대표적인 예는 스레드풀(ExecutorService)의 작업 큐이다. 작업 큐는 먼저 들어온 작업부터 처리한다.



![](https://t1.daumcdn.net/cfile/tistory/231B374B595F67F43A)

스레드풀 : 우리가 만든 어플리케이션에서 사용자로부터 들어온 요청을 작업큐에 넣고 스레드 풀은 작업 큐에 들어온 Task 일감을 미리 생성해놓은 Thread들에게 일감을 할당한다.   
[https://limkydev.tistory.com/55](https://limkydev.tistory.com/55)

### Stack
LIFO 자료구조를 구현한 클래스이다. 다음은 Stack 클래스의 주요 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/2705C14255FD0C5913)

### Queue
FIFO 자료구조를 구현한 클래스이다. 다음은 Queue 인터페이스에 정의되어 있는 메서드들이다.

![](https://t1.daumcdn.net/cfile/tistory/2457254255FD0C5E36)

Queue 인터페이스를 구현한 대표적인 클래스는 LinkedList이다. LinkedList는 List 인터페이스를 구현했기에 List 컬렉션이기도하다. 다음은 LinkedList 객체를 Queue 인터페이스 타입으로 변환한 것이다.

	public class Message {
		public String command;
		public String to;
	
		public Message(String command, String to) {
			this.command = command;
			this.to = to;
		}
	}

Queue를 이용한 코드


	import java.util.LinkedList;
	import java.util.Queue;

	public class QueueExample {
		public static void main(String[] args) {
			Queue<Message> messageQueue = new LinkedList<Message>();
		
			messageQueue.offer(new Message("sendMail", "홍길동"));
			messageQueue.offer(new Message("sendSMS", "신용권"));
			messageQueue.offer(new Message("sendKakaotalk", "홍두께"));
		
			while (!messageQueue.isEmpty()) {
				Message message = messageQueue.poll();
				switch (message.command) {
				case "sendMail" :
					System.out.println(message.to + "님에게 메일을 보냅니다.");
					break;
				case "sendSMS" :
					System.out.println(message.to + "님에게 메일을 보냅니다.");
					break;
				case "sendKakaotalk" :
					System.out.println(message.to + "님에게 메일을 보냅니다.");
					break;
				//편의상 default 생략
				}
			}
		}
	}

실행결과

	홍길동님에게 메일을 보냅니다.
	신용권님에게 메일을 보냅니다.
	홍두께님에게 메일을 보냅니다.

## 동기화된 컬렉션
컬렉션 프레임워크의 대부분의 클래스들은 싱글 스레드 환경에서 사용할 수 있도록 설계되었다. 그렇기 때문에 여러 스레드가 동시에 컬렉션에 접근한다면 의도치 않게 요소가 변경될 수 있는 불안전한 상태이다. Vector와 Hashtable은 동기화(synchronized) 메서드로 구성되어 있기 때문에 멀티 스레드 환경에서 안전하게 요소를 처리할 수 있지만, ArrayList, HashSet, HashMap은 동기화된 메서드로 구성되어 있지 않아 멀티 스레드 환경에서 안전하지 않다.
경우에 따라서는 ArrayList, HashSet, HashMap를 싱글 스레드환경에서 사용하다가 멀티 스레드 환경으로 전달할 필요가 있을 것이다. 이런 경우를 대비해 컬렉션 프레임워크는 비동기화된 메서드를 동기화된 메서드로 래핑하는 Collections의 synchronizedXXX() 메서드를 제공하고 있다. 매개 값으로 비동기화된 컬렉션을 대입하면 동기화된 컬렉션을 리턴한다.

>자바에서 지원하는 Synchronized(동기화) 키워드는 여러개의 스레드가 한개의 자원을 사용하고자 할 때, 현재 데이터를 사용하고 있는 해당 스레드를 제외하고 나머지 스레드들은 데이터에 접근 할 수 없도록 막는 개념입니다.
>[동기화 관련 개념사이트 1](https://coding-start.tistory.com/68)
>[동기화 관련 개념사이트 2](https://tourspace.tistory.com/54)

![](https://t1.daumcdn.net/cfile/tistory/225CC64255FDF84B23)



![](https://t1.daumcdn.net/cfile/tistory/2767894255FDF84D1C)
![](https://t1.daumcdn.net/cfile/tistory/255F2F4255FDF84F20)
![](https://t1.daumcdn.net/cfile/tistory/2263814255FDF8521F)


## 병렬처리를 위한 컬렉션
동기화된 컬렉션은 멀티 스레드 환경에서 하나의 스레드가 요소를 안전하게 처리하도록 도와주지만, 전체 요소를 빠르게 처리하지는 못한다. 하나의 스레드가 요소를 처리할 때 전체 잠금이 발생하여 다른 스레드는 대기 상태가 된다. 그렇기 때문에 멀티 스레드가 병렬적으로 컬렉션의 요소들을 처리할 수 없다. 
자바는 멀티 스레드가 컬렉션의 요소를 병렬적으로 처리할 수 있도록 특별한 컬렉션을 제공하고 있다. java.util.concurrent 패키지의 ConcurrentHashMap(Map 구현 클래스)과 ConcurrentLinkedQueue(Queue 구현 클래스)이다. 
ConcurrentHashMap을 사용하면 스레드에 안전하면서도 멀티 스레드가 요소를 병렬적으로 처리할 수 있다. 이것이 가능한 이유는 ConcurrentHashMap은 부분(segment) 잠금을 사용하기 때문이다.

>**전체 잠금** : 컬렉션에 10개의 요소가 저장되어있을 때, 1개를 처리할 동안 전체 10개의 요소를 다른 스레드가 처리하지 못하도록 하는 것  
>**부분 잠금** : 처리하는 요소가 포함된 부분만 잠금하고 나머지 부분은 다른 스레드가 변경할 수 있도록 하는 것이 부분 잠금이다.

	Map<K, V> map = new ConcurrentHashMap<K, V>();

ConcurrentLinkedQueue는 락-프리(lock-free) 알고리즘을 구현한 컬렉션이다. 락-프리 알고리즘은 여러 개의 스레드가 동시에 접근할 경우, 잠금을 사용하지 않고도 최소한 하나의 스레드가 안전하게 요소를 저장하거나 얻도록 해준다.

	Queue<E> queue = new ConcurrentQueue<E>();
 





참고  

+  https://dipa92.tistory.com/entry/JAVA-Collection-Framework-List-%EC%BB%AC%EB%A0%89%EC%85%98ArrayList-Vector-LinkList
+  이것이 자바다
+  https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Arrays.html#asList(T...)
+  https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Thread.html
+  https://palpit.tistory.com/654
+  https://m.blog.naver.com/PostView.nhn?blogId=qkrghdud0&logNo=220699616483&proxyReferer=https:%2F%2Fwww.google.com%2F
+  https://www.slideshare.net/ssuser34b989/collection-framework-152443499
+  https://palpit.tistory.com/entry/Java-%EC%BB%AC%EB%A0%89%EC%85%98-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-LIFO%EC%99%80-FIFO-%EC%BB%AC%EB%A0%89%EC%85%98?category=843239
+  http://ehpub.co.kr/category/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-with-c/page/33/
+  https://limkydev.tistory.com/55
+  https://palpit.tistory.com/entry/Java-%EC%BB%AC%EB%A0%89%EC%85%98-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-%EB%8F%99%EA%B8%B0%ED%99%94-%EB%B3%91%EB%A0%AC-%EC%B2%98%EB%A6%AC?category=843239
+  https://coding-start.tistory.com/68
+  https://tourspace.tistory.com/54
+  https://farmerkyh.tistory.com/844