# 스트림(stream)
스트림은 자바 8부터 추가된 컬렉션(배열 포함)의 저장 요소를 하나씩 참조해서 람다식(함수적 스타일)으로 처리할 수 있도록 해주는 반복자이다. 
> 외부 반복자(external iterator) : 개발자가 코드로 직접 컬렉션의 요소를 반복해서 가져오는 코드 패턴(for문, Iterator을 이용하는 while문등)

반면 **내부 반복자(internal iterator)** 는 컬렉션 내부에서 요소들을 반복시키고, 개발자는 요소당 처리해야 할 코드만 제공하는 코드패턴이다. 
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbIkiar%2Fbtqv8ZcbOZx%2FklC5KFkgrq14Y5r7veUE11%2Fimg.png)
내부 반복자를 사용해서 얻는 이점은 개발자가 요소 처리에만 더 몰두할 수 있는 것도 있지만, 가장 중요한 건 요소의 병렬 처리가 컬렉션 내부에서 진행되므로 병렬 처리부분에서 굉장히 효율적이다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvbqSH%2Fbtqv95iF68r%2FR5oCBRzEyKjtY2CSscaSq0%2Fimg.png)
 
### 스트림 특징
Stream은 Iterator와 비슷한 역할을 하는 반복자이지만, 람다식으로 요소 처리 코드를 제공하는 점과 내부 반복자를 사용하므로 병렬 처리가 쉽다는 점 그리고 중간 처리와 최종 처리 작업을 수행하는 점에서 많은 차이를 가지고 있다. 

	import java.util.Arrays;
	import java.util.List;
	import java.util.Iterator;
	import java.util.stream.Stream;

	public class IteratorVsStreamExample {

		public static void main(String[] args) {
			List<String> list = Arrays.asList("홍길동", "신용권", "감자바");
			
			//Iterator이용
			Iterator<String> iterator = list.iterator();
			while (iterator.hasNext()) {
				String name = iterator.next();
				System.out.println(name);
			}
			
			System.out.println();
			
			//Stream 이용
			Stream<String> stream = list.stream();
			stream.forEach(name -> System.out.println(name));
	
		}
	}	

+ 람다식으로 요소 처리 코드를 제공한다. 
> Stream이 제공하는 대부분의 요소 처리 메서드는 함수적 인터페이스 매개 타입을 가지기 때문에 람다식 또는 메서드 참조를 이용해서 요소 처리 내용을 매개값으로 전달 가능

+ 내부 반복자를 사용하므로 병렬 처리가 쉽다.
+ 스트림은 중간 처리와 최종 처리를 할 수 있다.

## 스트림 파이프라인
![](https://mblogthumb-phinf.pstatic.net/20160120_14/rain483_1453259635244Tl0Uh_JPEG/6.png?type=w2)

스트림은 데이터의 필터링, 매핑, 정렬, 그룹핑 등의 중간 처리와 합계, 평균, 카운팅, 최대값, 최소값 등의 최종 처리를 파이프라인(pipelines)으로 해결한다. 파이프라인은 여러 개의 스트림이 연결되어 있는 구조이다. 
중간 스트림은 생성될 때 바로 동작하는 것이 아니라 최종 처리가 시작되기 전까지 지연(lazy)된다. 최종 처리가 시작되면 비로소 컬렉션의 요소가 하나씩 중간 스트림에서 처리되고 최종 처리까지 오게 된다.

![](https://mblogthumb-phinf.pstatic.net/20160506_22/qkrghdud0_1462512138158d9EFG_PNG/%C1%DF%B0%A3%C3%B3%B8%AE.png?type=w2)
![](https://mblogthumb-phinf.pstatic.net/20160506_50/qkrghdud0_1462512005388vlMlg_PNG/%C3%D6%C1%BE%C3%B3%B8%AE.png?type=w2)

### Optional Class
Optional 클래스는 값을 저장하는 값 기반 클래스(value-based class)이다. 이 객체에서 값을 얻기 위해선 get, getAsInt, getAsDouble, getAsLong 메서드를 호출하면 된다. 또, Optional 클래스는 단순히 집계 값을 저장하는 것이 아니라, 집계 값이 존재하지 않을 경우 디폴트 값을 설정할 수도 있고, 집계 값을 처리하는 Consumer도 등록할 수 있다.

	import java.util.ArrayList;
	import java.util.List;
	import java.util.OptionalDouble;

	public class Example {
		public static void main(String[] args) {
			List<Integer> list = new ArrayList<>();
			
			/*//예외발생
			double avg = list.stream()
					.mapToInt(Integer :: intValue)
					.average()
					.getAsDouble();
			*/
			
			//방법1
			OptionalDouble optional = list.stream()
					.mapToInt(Integer :: intValue)
					.average();
			if(optional.isPresent()) {
				System.out.println("방법1_평균 : " + optional.getAsDouble());
			} else {
				System.out.println("방법1_평균 : 0.0");
			}
			
			//방법2
			double avg = list.stream()
					.mapToInt(Integer :: intValue)
					.average()
					.orElse(0.0);
			System.out.println("방법2_평균 : " + avg);
			
			//방법3
			list.stream()
			    .mapToInt(Integer :: intValue)
			    .average()
			    .ifPresent(a -> System.out.println("방법3_평균 : " + a));
		}
	}


	출력창
	방법1_평균 : 0.0
	방법2_평균 : 0.0

## 커스텀 집계(reduce())
스트림은 기본 집계 메서드인 sum(), average(), count(), max(), min()을 제공하지만, 프로그램화해서 다양한 집계 결과물을 만들 수 있도록 reduce()메서드도 제공한다. 

![](https://mblogthumb-phinf.pstatic.net/20160506_66/qkrghdud0_1462521278962Grhk2_PNG/reduce%28%29.png?type=w2)

	int sum = studentList.stream();
		.map(Student :: getScore)
		.reduce((a,b) -> a + b)
		.get();							//요소 없을 경우 NoSuchElementException이 발생한다.

	int sum = studentList.stream();
		.map(Student :: getScore)
		.reduce(0, (a,b) -> a + b);		//요소 없을 경우 디폴트 값인 0을 리턴함.

## 수집(collect)
스트림은 요소들을 필터링 또는 매핑한 후 요소들을 수집하는 최종 처리 메서드인 collect()를 제공하고 있다. 

![](https://mblogthumb-phinf.pstatic.net/20160506_179/qkrghdud0_1462523607047pvMQ9_PNG/Collectors_%C5%AC%B7%A1%BD%BA_%C1%A4%C0%FB_%B8%DE%BC%D2%B5%E5.png?type=w2)

	import java.util.Arrays;
	import java.util.HashSet;
	import java.util.Set;
	import java.util.List;
	import java.util.stream.Collectors;			//Collector인터페이스 구현한 클래스

	public class ToListExample {
		public static void main(String[] args) {
			List<Student> totalList = Arrays.asList(
					new Student("홍길동", 10, Student.Sex.MALE),
					new Student("김수애", 6, Student.Sex.FEMALE),
					new Student("신용권", 10, Student.Sex.MALE),
					new Student("박수미", 6, Student.Sex.FEMALE)
					);
	
		//남자만 묶어 List생성
		List<Student> maleList = totalList.stream()
				.filter(s -> s.getSex() == Student.Sex.MALE)
				.collect(Collectors.toList());
		maleList.stream()
		.forEach(s -> System.out.println(s.getName()));
		
		System.out.println();
		
		//여자만 묶어 Set 생성
		Set<Student> femaleSet = totalList.stream()
				.filter(s -> s.getSex() == Student.Sex.FEMALE)
				.collect(Collectors.toCollection(HashSet :: new));		//toSet()메서드도 가능
		
		femaleSet.stream()
		.forEach(s -> System.out.println(s.getName()));
		
		}
	}

참고 

+ https://cornswrold.tistory.com/293
+ 이것이 자바다
+ https://m.blog.naver.com/PostView.nhn?blogId=qkrghdud0&logNo=220702640712&proxyReferer=https:%2F%2Fwww.google.com%2F
+ https://m.blog.naver.com/PostView.nhn?blogId=qkrghdud0&logNo=220702812522&proxyReferer=https:%2F%2Fwww.google.com%2F