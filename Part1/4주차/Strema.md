# Java Stream API
수많은 데이터의 흐름 속에서 각각의 값을 원하는 형태로 가공하여 최종 소비자에게 제공하는 것이 stream의 역할이다.

## 1. Stream은 무엇인가?
stream은 Array, Collections와 같이 연속된 형태의 객체이다. 하지만 자료구조는 아니다. 위와 같은 데이터를 입력으로 받아 메소드로 처리할 뿐이다. stream은 입력받은 원래의 자료구조를 바꾸지는 못한다. 대신, 파이프라인 형태로 연결된 메소드의 결과를 제공한다. stream은 입력받은 원래의 자료구조를 바꾸지는 못한다. 대신, 파이프라인 형태로 연결된 메소드의 결과를 제공한다.
목적은 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 것.

## 2. Stream만들기
### 2-1 스트림의 생성

**배열 스트림**

배열은 다음과 같이 Arrays.stream 메소드를 사용합니다.   
객체 배열로 부터 스트림 생성하기

    Stream<T> Stream.of(T... values)
    Stream<T> Stream.of(T[])
    Stream<T> Arrays.stream(T[]);
    Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive)//배열의 일부로만 스트림 만들기

예)

    Stream<String> strStream = Stream.of("a", "b", "c");
    Stream<String> strStream = stream.of(new String[]{"a", "b", "c"});
    Stream<String> strStream = Arrays.stream(new String[]{"a", "b", "c"});
    Stream<String> strStream = Arrays.stream(new String[]{"a", "b", "c"}, 0, 3); //index가 0,1,2까지 스트림으로 만들어짐

기본형 스트림은 Stream<Integer>과 똑같으므로 생성도 똑같이 할 수 있다.

**컬렉션 스트림**

컬렉션 타입(Collection, List, Set)의 경우 인터페이스에 추가된 디폴트 메소드 stream 을 이용해서 스트림을 만들 수 있습니다.

    public interface Collection<E> extends Iterable<E> {
        default Stream<E> stream() {
             return StreamSupport.stream(spliterator(), false);
             }
         // ...
     }

예)

    List<String> list = Arrays.asList("a", "b", "c");
    Stream<String> stream = list.stream();
    Stream<String> parallelStream = list.parallelStream(); // 병렬 처리 스트림

**파일과 비어있는 스트림**

파일을 소스로하는 스트림 생성하기

    Stream<Path> Files.list(Path dir) // Path는 파일 또는 디렉토리
    Stream<String> Files.lines(Path path)//파일의 내용을 한줄씩읽어서 스트림 요소로 만든다.
    Stream<String> Files.lines(Path path, Charset cs)
    Stream<String> lines() // BufferedReader클래스의 메서드 : 파일 읽을때 쓰는 클래스

비어 있는 스트림(empty streams)도 생성할 수 있습니다. 빈 스트림은 요소가 없을 때 습니다.null 대신 사용할 수 있습니다.

    Stream emptyStream = Stream.empty(); // 빈 스트림을 생성해서 반환한다.
    long count = emptyStream.coutn(); // 0

밑의 코드는 streamOf메서드 예시코드

    public Stream<String> streamOf(List<String> list) {
        return list == null || list.isEmpty() ? Stream.empty() : list.stream();
    }

**난수 스트림**

난수를 요소로 갖는 스트림 생성하기

    IntStream intStream = new Random().ints();//무한스트림
    intStream.limit(5).forEach(System.out::println);//5개의 요소만 출력
    IntStream intStream = new Random().ints(5); //크기가 5인 난수 스트림 반환


지정된 범위의 난수를 요소로 갖는 스트림을 생성하는 메서드(Random클래스)

    Int Stream ints(int begin, int end) //무한 스트림
    LongStream longs(long begin, long end)
    DoubleStream doubles(double begin, double end)

    IntStream ints(long streamSize, int begin, int end) //유한 스트림
    LongStream longs(long streamSize, long begin, long end)
    DoubleStream doubles(long streamSize, double begin, double end)


특정범위의 정수 스트림 만들기

특정 범위의 정수를 요소로 갖는 스트림 생성하기

    IntStream IntStream.range(int begin, int end)// end 포함안함
    IntStream IntStream.rangeClosed(int begin, int end) // end까지 포함

**람다식 스트림 만들기**

람다식을 소스로 하는 스트림 생성하기

    static <T> Stream<T> iterate(T seed, UnaryOperator<T> f) //이전 요소에 종속적
    static <T> Stream<T> generate(Supplier<T> s) // 이전 요소에 독립적

iterate()는 이전 요소를 seed로 해서 다음 요소를 계산한다.

    Stream<Integer> evenStream = Stream.iterate(0,n -> n+2); // 0,2,4,6, ...

generate()는 seed를 사용하지 않는다.

    Stream<Double> randomStream = Stream.generate(Math::random());
    Stream<Integer> oneStream = Stream.generate1(()->1);

### 2-2 스트림의 중간연산

**스트림 자르기 - skip(), limit()**

    Stream<T> skip(long n) // 앞에서부터 n개 건너뛰기
    Stream<T> limit(long maxSize) // maxSize 이후의 요소는 잘라냄

**스트림 요소 걸러내기 - filter(), distinct()**

    Stream<T> filter(Predicate<? super T> predicate) // 조건에 맞지 않는 요소 제거
    Stream<T> distict()//중복 제거

**스트림 정렬하기 - sorted()**

    Stream<T> sorted() //스트림 요소의 기본 정렬(Comparable)로 정렬
    Stream<T> sorted(Comparator<? super T> comparator) //지정된 Comparator로 정렬

<img src="C:\Users\신완수\자바 공부\자바 문자열 스트림 정렬 캡쳐.PNG" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="문자열 스트림"></img><br/>

Comparator의 comparing()에 대해서 알아보자.

    comparing(Function<T, U> keyExtractor)
    comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)

예제

    studentStream.sorted(Comparator.comparing(Student::getBan)) // 반별로 정렬
    .forEach(System.out::println);

추가 정렬 기준을 제공할 때는 thenComparing()사용

    thenComparing(Comparator<T> other)
    thenComparing(Function<T, U> keyExtractor)
    thenComparing(Function<T, U> keyExtractor, Comparator<U> keyComp)

**스트림의 요소 변환하기 - map()**

    Stream<R> map(Function<? super T, ? extends R> mapper) // Stream<T> -> Stream<R>

    Stream<File> fileStream = Stream.of(New File("Ex1.java"), new File("Ex1"), newFile("Ex1.bak"), new File("Ex2.java"), New File("Ex1.txt"));
    Stream<String> filenameStream = fileStream.map(File::getName);
    filenameStream.forEach(System.out::println); // 스트림의 모든 파일의 이름을 출력
    Stream<File> -> Stream<String>

**스트림의 요소를 소비하지 않고 엿보기 - peek()**

    Stream<T> peek(Consumer<? super T> action) // 중간 연산(스트림요소를 소비X)
    void forEach(Consumer<? super T> action) // 최종 연산(스트림요소를 소비O)


**스트림의 스트림을 스트림으로 변환 - flatMap()**

    Stream<String[]> strArrStrm = Stream.of(new String[]{"abc", "def", "ghi"},
    new String[]{"ABC", "GHI", "JKLMN"});

    Stream<Stream<String>> strStrStrm = strArrStrm.map(Arrays::stream);// stream의 stream을 생성
    Stream<String> strStrStrm = strArrStrm.flatMap(Arrays::stream); // Arrays.stream(T[])

**Optional< T >**

T타입 객체의 래퍼클래스 - Optional<**T**>

    public final class Optional<T> {
        private final T value; //T타입의 참조변수
        ...
    }

모든 종류의 객체 저장가능 null도 가능

쓰는 목적 :
1. null을 직접적으로 다루면 위험하므로(NullPointerException) 간접적으로 다루기 위해서 쓴다.
2. null체크를 안해도 된다. null체크를 할려면 if문을 써야하는데 코드가 지저분해진다.

Optional<**T**> 객체 생성하기

Optional<**T**> 객체를 생성하는 다양한 방법

    String str = "abc";
    Optional<String> optVal = Optional.of(str);
    Optional<String> optVal = Optional.of("abc");
    Optional<String> optVal = Optional.of(null); //NullPointerException발생
    Optional<String> optVal = Optional.ofNullable(null) //OK

str이 null이라도 optval은 str의 주소를 가리키기 때문에 optival이 null이 되진 않는다. 
null대신 빈 Optional<**T**>객체를 사용하자

    Optional<String> optVal = null; //널로 초기화, 바람직 하지 않다.
    Optional<String> optVal = Optional.<String>empty(); // 빈 객체로 초기화 <String> 생략가능

Optional<**T**> 객체의 값 가져오기

Optional객체의 값 가져오기 - get(), orElse(), orElseGet(), orElseThrow()

    Optional<String> optVal = Optional.of("abc")
    String str1 = optVal.get(); // optVal에 저장된 값을 반환, null이면 예외 발생
    String str2 = optVal.orElse(""); //optVal에 저장된 값이 null일 때는 ""를 반환
    String str3 = optVal.orElseGet(String::new); // 람다식 사용가능 () -> new String()
    String str4 = optVal.orElseThrow(NullPointerException::new); // 널이면 예외발생


isPresent() - Optional객체의 값이 null이면 false, 아니면 true를 반환

    if(Optional.ofNullable(str).isPresent()) {
        System.out.println(str);
    }

ifPresent(Consumer) - 널이 아닐때만 작업 수행, 널이면 아무 일도 안한다.

    Optional.ofNullable(str).ifPresent(System.out::println);

기본형 값을 감싸는 래퍼클래스 - OptionalInt, OptionalLong, OptionalDouble

성능을 높이기 위해서 사용

    public final class OptionalInt {
        ...
        private final boolean isPresent; //값이 저장되어 있으면 true
        private final int value; // int타입의 변수

OptionalInt의 값 가져오기

<img src="C:\Users\신완수\자바 공부\자바 옵셔널 래퍼 클래스 캡쳐.PNG" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="옵셔널 래퍼"></img><br/>

빈 Optional객체와 비교

    OptionalInt opt = OptionalInt.of(0); //OptionalInt에 0을 저장
    OptionalInt opt2 = OptionalInt.empty(); // OptionalInt에 0을 저장
    System.out.println(opt.isPresent()); //true
    System.out.println(opt2.isPresent()); //false
    System.out.println(opt.equals(op2)); //false

### 2-3 최종 연산

**forEach()**

    void forEach(Consuber<? super T> action) //병렬스트림인 경우 순서가 보장되지 않음.
    void forEachOrdered(Consumer<? super T> action) //병렬스트림인 경우에도 순서가 보장됨.

    IntStream.range(1, 10).sequential().forEach(System.out::print) //123456789
    IntStream.range(1, 10).sequential().forEachOrdered(System.out::print) //123456789
    sequential은 직렬 스트림

    IntStream.range(1, 10).parallel().forEach(System.out::print) //683295714 순서 보장 X
    IntStream.range(1, 10).parallel().forEachOrdered(System.out::print) //123456789


**조건검사 - allMatch(), anyMatch(), noneMatch()**

    boolean allMatch(Predicate<? super T> predicate) //모든 요소가 조건을 만족시키면 true
    boolean anyMatch(Predicate<? super T> predicate) // 한 요소라도 조건을 만족시키면 true
    boolean noneMatch(Predicate<? super T> predicate) // 모든 요소가 조건을 만족시키지 않으면 true

**조건에 일치하는 요소 찾기 - findFirst(), findAny()**

    Optional<T> findFirst() //첫 번째 요소를 반환, 순차 스트림에 사용
    Optional<T> findAny() //아무거나 하나를 반환, 병렬 스트림에 사용

Optional인 이유는 결과가 null일 수 도 있기 때문이다.

    Optional<Student> result = stuStream.filter(s -> s.getTotalScore() <= 100).findFirst();
    Optional<Student> result = parallelStream.filter(s -> s.getTotalScore() <= 100).findAny();

**스트림의 요소를 하나씩 줄여가며 누적연산 수행 - reduce()**

    Optional<T> reduce(BinaryOperator<T> accumulator)
    T reduce(T identity, BinaryOperator<T> accumulator)
    U reduce(U identity, BiFunction<U, T, U> accumulator, BinaryOperator<U> combiner)

identity - 초기값
accumulator - 이전 연산결과와 스트림의 요소에 수행할 연산
combiner - 병렬처리된 결과를 합치는데 사용할 연산 ( 병렬 스트림 )

    int count = intStream.reduce(0, (a,b) -> a + 1); //count()
    int sum = intStream.reduce(0, (a,b) -> a + b); //sum()

reduce는

    int a = identity;

    for(int b : stream)
    a = a+b; // sum()

이런식으로 돌아감

    int max = intStream.reduce(Integer.MIN_VALUE, (a,b) -> a > b ? a : b); //max()
    int min = intStream.reduce(Integer.Max_VALUE, (a,b) -> a < b ? a : b); //min()

**collect()는 Collector를 매개변수로 하는 스트림의 최종연산**

    Object collect(Collector collector) // Collector(interface)를 구현한 객체를 매개변수로
    Object collect(Supplier supplier, BiConsumer accumulator, BiConsumer combiner) // 잘 안쓰임

reduce는 전체에 대한거 collect는 그룹별

Collector는 수집(collect)에 필요한 메서드를 정의해 놓은 인터페이스

    public interface Collector<T, A, R> { //T(요소)를 A에 누적한 다음, 결과를 R로 변환해서 반환
        Supplier<A> supplier(); //StringBuilder::new 누적할 곳
        BiConsumer<A, T> accumulator(); // (sb, s) -> sb.append(s) 누적방법
        BinaryOperator<A> combiner(); // (sb1, sb2) -> sb1.append(sb2) 결합방법(병렬)
        Function<A, R> finisher(); // sb -> sb.toString() 최종변환
        Set<Characteristics> characteristics(); // 컬렉터의 특성이 담긴 Set을 반환
        ...
    }

Collectors클래스는 다양한 기능의 컬렉터(Collector를 구현한 클래스)를 제공
Collector 인터페이스를 직접구현 하는 일은 거의 없음.

    변환 - mapping(), toList(), toSet(), toMap(), toCollection(), ...
    통계 - counting(), summingInt(), averagingInt(), maxBy(), minBy(), summarizingInt(), ...
    문자열 결합 - joining()
    리듀싱 - reducing()
    그룹화와 분할 - groupingBy(), partitioningBy(), collectingAndThen()

**스트림을 컬렉션으로 변환 - toList(), toSet(), toMap(), toCollection()**

    List<String> names = stuStream.map(Student::getName) // Stream<Strudent> -> Stream<String>
    .collect(Collectors.toList()); // Stream<String> -> List<String>
    ArrayList<String> list = names.stream()
    .collect(Collectors.toCollection(ArrayList::new); // Stream<String> -> ArrayList<String>

    Map<String, Person> map = personStream
    .collect(Collectors.toMap(p -> p.getRegId(), p->p)); // Stream<Person> -> Map<String, Person>

**스트림을 배열로 변환 - toArray()**

    Student[] stuNames = studentStream.toArray(Student[]::new); // OK
    Student[] stuNames = studentStream.toArray(); // 에러.빈괄호면 Objcet[]반환

**스트림의 통계정보 제공 - counting(), summingInt(), maxBy(), minBy(), ...**

    long count = stuStream.count();
    long count = stuStream.collect(counting()); // Collectors.counting()

첫번째 줄 코드는 전체를 카운트하는거 두번째 줄은 그룹별로 카운트 가능

    long totalScore = stuStream.mapToInt(Student::getTotalScore).sum(); // IntStream의 sum()
    long totlaScore = stuStream.collect(summingInt(Student::getTotalScore));


    OptionalInt topScore = studentStream.mapToInt(Student::getTotalScore).max();

    Optional<Student> topStudent = stuStream
    .max(Comparator.comparingInt(Student::getTotalScore));

    Optional<Student> topStudent = stuStream
    .collect(maxBy(Comparator.comparingInt(Student::getTotalScore)));

    Object[] stuNames = studentStream.toArray(); // OK.


**스트림을 리듀싱 - reducing()**

reduce()는 전체 리듀싱
reducing()은 그룹별 리듀싱


    Collector reducing(BinaryOperator<T> op)
    Collector reducing(T identity, BinaryOperator<T> op)
    Collector reducing(U identity, Function<T,U> mapper, BinaryOperator<U> op) // map+reduce 변환작업 필요할때


**문자열 스트림의 요소를 모두 연결 - joining**

    String studentNames = stuStream.map(Student::getName).collect(joining());
    String studentNames = stuStream.map(Student::getName).collect(joining(",")); // 구분자
    String studentNames = stuStream.map(Student::getName).collect(joining(",", "[", "]"));
    String studentInfo = stuStream.collect(joining(",")); //Student의 toString()으로 결합


**스트림의 그룹화와 분할**

partitioningBy()는 스트림을 2분할 한다.

    Collector partitioningBy(Predicate predicate)
    Collector partitioningBy(Predicate predicate, Collector downstream)

groupingBy()는 스트림을 n분할한다.

    Collector groupingBy(Function classifier)
    Collector groupingBy(Function classifier, Collector downstream)
    Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)

Map이랑 같이 많이 쓴다.

    Map<Integer, List<student>> stuByBan = stuStream //학생을 반별로 그룹화
    .collect(groupingBy(Student::getBan, toList())); //toList() 생략가능
    Map<Integer, Map<Integer, List<Student>>> stuByHakAndBan = stuStream //다중 그룹화
    .collect(groupingBy(Student::getHak, //1. 학년별 그룹화
    groupingBy(Student::getBan) //2. 반별 그룹화
    ));

스트림의 변환
<img src="C:\Users\신완수\자바 공부\자바 스트림 변환1.PNG" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="스트림 변환1"></img><br/>
<img src="C:\Users\신완수\자바 공부\자바 스트림 변환2.PNG" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="스트림 변환2"></img><br/>

## 2. 스트림의 특징

1. 원본 데이터를 바꾸지 못하는 특성 덕분에 side effect를 제거할 수 있다.
2. 스트림은 데이터 소스로 부터 데이터를 읽기만 할 뿐 변경하진 않는다.
3. 스트림은 Iterator처럼 일회용이다.(필요하면 다시 생성해야함.)

예

    strStream.forEach(System.out::println);// 최종연산
    int numOfStr = strStream.count(); // 이미 스트림이 닫힘.

4. 최종 연산이 전까지 중간연산이 수행되지 않는다. - 지연된 연산

예

    IntStream intStream = new Random().ints(1, 46); //1~45범위의 무한 스트림
    IntStream.distinct().limit(6).sorted() //중간연산(지연된 연산때문에)
    .forEach(i -> System.out.print(i + ",")); //최종연산

5. Stream은 작업을 내부 반복으로 처리한다.

**외부 반복자(external iterator)**
개발자가 코드로 직접 컬렉션 요소를 반복해서 요청하고 가져오는 코드 패턴

**내부 반복자(internal iterator)**
컬렉션 내부에서 요소들을 반복시키고 개발자는 요소당 처리해야할 코드만 제공하는 코드 패턴

==> 개발자는 요소 처리 코드에만 집중, 코드가 간결해짐.

예

    for(String str : strList) {
    System.out.println(str);
    }//stream X

    stream.forEach(System.out::println); //stream O


    void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action); // 매개변수 Null체크
        for(T t : src) { //내부반복 (for문을 메서드 안으로 넣음)
        action.accept(t);
        }
    }

6. 스트림의 작업을 병렬로 처리 - 병렬 스트림

**병렬 처리**

한가지 작업을 서브 작업으로 나누고, 서브 작업들을 분리된 스레드에서 병렬적으로 처리한 후, 서브 작업들의 결과들을 최종 결합하는 방법

예

    Stream<String> strStream = Stream.of("dd", "aaa", "CC", "cc", "b");
    int sum = strStream.parallel() // 병렬 스트림으로 전환 (속성만 변경) 반대로는 sequent() 메서드 이용
    .mapToInt(s -> s.length()).sum(); // 모든 문자열의 길이의 합

**병렬 처리는 항상 빠르다?**

스트림 병렬 처리가 스트림 순차 처리보다 항상 실행 성능이 좋다고 판단해서는 안된다.

병렬 처리에 영향을 미치는 3가지 요인

* 요소의 수와 요소당 처리 시간

컬렉션에 요소의 수가 적고 요소당 처리 시간이 짧으면 순차 처리가 오히려 병렬 처리보다 빠를 수 있다. 병렬 처리는 스레드풀 생성, 스레드 생성이라는 추가적인 비용이 발생하기 때문이다.

* 스트림 소스의 종류

ArrayList, 배열은 랜덤 액세스를 지원(인덱스로 접근)하기 때문에 포크 단계에서 쉽게 요소를 분리할 수 있어 병렬 처리 시간이 절약된다. 반면에 HashSet, TreeSet은 요소를 분리하기가 쉽지 않고, LinkedList는 랜덤 액세스를 지원하지 않아 링크를 따라가야 하므로 역시 요소를 분리하기가 쉽지 않다. 또한 BufferedReader.lines()은 전체 요소의 수를 알기 어렵기 때문에 포크 단계에서 부분요소로 나누기 어렵다. 따라서 이들 소스들은 ArrayList, 배열 보다는 상대적으로 병렬 처리가 늦다.

* 코어(Core)의 수

싱글 코어 CPU일 경우에는 순차 처리가 빠르다. 병렬 처리를 할 경우 스레드의 수만 증가하고 번갈아 가면서 스케쥴링을 해야하므로 좋지 못한 결과를 준다. 코어의 수가 많으면 많을 수록 병렬 작업 처리 속도는 빨라진다.

7. 기본형 스트림 - IntStream, LongStream, DoubleStream

- 오토박싱&언박싱의 비효율이 제거됨(Stream<Integer>대신 IntStream사용)

- 숫자와 관련된 유용한 메서드를 Stream<T>보다 더 많이 제공(Stream<T>는 count만 있는데, 기본형 스트림은 count, sum, average가 있다.)

**오토박싱 : 기본형 -> 참조형 변환하는거, 언박싱 : 참조형 -> 기본형 변환하는거**

제네릭 타입은 참조형밖에 안되므로 기본형을 참조형으로 바꾸는 오토박싱이 필요하고 이것을 계산할때는 기본형으로 바꿔야 하므로 다시 언박싱을 해야한다. 이러한 작업을 덜어주기 위해 기본형 스트림을 지원.

## 참고
* https://velog.io/@kskim/Java-Stream-API
* https://futurecreator.github.io/2018/08/26/java-8-streams/
* 자바의 정석 동영상 강의
* https://ict-nroo.tistory.com/43