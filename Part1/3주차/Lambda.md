# 람다식
JAVA의 Lambda는 메소드를 하나의 식(Expression)으로 표현한 것이다. 익명 메소드 생성 문법이라고 할 수 있다. 메소드를 지칭하는 명칭(메소드 명)없이 구현부만으로 선언하는 것이다. 하지만 JAVA의 메소드는 메소드 자체로 혼자 선언하여 쓰일 수 없다. 무조건 Class 구성멤버로 선언되어야 한다. 람다식을 통해서 생성되는 것은 메소드 자체가 아닌 실행문(메소드)을 가진 객체이다. 람다식은 일반적인 객체가 아닌 인터페이스를 구현한 익명구현객체를 생성한다.

## 함수적 인터페이스

람다식을 이용하기 위해서는 구현할 인터페이스가 필요하다. 람다식으로 구현하기 위한 인터페이스(타겟 타입이 될 인터페이스)에는 조건이 하나있는데 한개의 추상메소드만 가지고 있어야 한다는 것이다. 그렇게 되면 컴파일러가 해당 람다식이 타겟타입의 어떤 메소드를 구현 할 것인지 모르기 때문이다. 그리고 이 인터페이스를 함수적 인터페이스(functionalInterface)라고 한다.      
즉, 함수구현 전용 인터페이스라고 한다. @FunctionalInterface 어노테이션으로 이런 함수적 인터페이스를 명시할 수 있다. 어노테이션을 붙히고 2개 이상의 추상 메소드를 만들면 에러가 발생한다.

## 람다식의 기본 구조    

소괄호는 구현한 함수의 인자를 그리고 화살표 다음에 중괄호에는 구현할 함수 몸체를 넣어주면 된다.     
람다는 많은 생략 기법을 사용한다.

1. 람다식 매개인자의 자료형은 생략이 가능하다.

2. 람다식의 매개인자가 한개인 경우 소괄호를 생략이 가능하다

3. 람다식의 함수몸체에 실행문이 한 개인 경우 중괄호를 생략이 가능하다.

4. 람다식의 함수몸체에 실행문이 한개이고, 그 실행문이 return문일 경우 함수의 몸체를 감싸는 중괄호와 return을 생략 할 수 있다.(단, return을 생략 안하고 중괄호만 생략 불가)

## 람다식의 사용 예제
람다식을 사용 안할때

    Person rin = new Person();
    rin.hi(new Say() {
        public int someting(int a, int b) {
        System.out.println("My Name is Coding-Factory");
        System.out.println("Nice to meet you");
        System.out.println("parameter number is "+a+","+b);
        return 7;
        }
    });

람다식을 사용 할때

    Person rin = new Person();
    rin.hi((a,b) ->{
        System.out.println("This is Coding-Factory!");
        System.out.println("Tank you Lamda");
        System.out.println("parameter number is "+a+","+b);
        return 7;
    });
    
## 람다식의 장점

1. 코드를 간결하게 만들 수 있다.
2. 코드가 간결하고 식에 개발자의 의도가 명확히 드러나므로 가독성이 향상됩니다.
3. 함수를 만드는 과정없이 한번에 처리할 수 있기에 코딩하는 시간이 줄어듭니다.
4. 병렬프로그래밍이 용이합니다.

## 람다식의 단점

1. 람다를 사용하면서 만드는 무명함수는 재사용이 불가능합니다.
2. 디버깅이 다소 까다롭습니다.
3. 람다를 남발하면 코드가 지저분해질 수 도 있다. (비슷한 함수를 계속 중복 생성할 가능성이 높다.)
4. 재귀로 만들경우 다소 부적합한 경우가 있다.       
람다식안에서 자신을 다시 호출하기가 용이하지 않다. 람다식안에서는 람다식을 가리키는 변수를 참조할수가 없다. 배열등의 트릭을 사용하면 가능하기는 한다.

예시

    import java.util.function.UnaryOperator;


    public class RecursiveLambdaExample {


        public static void main(String[] args) {


            UnaryOperator<Integer>[] fac = new UnaryOperator[1];

            fac[0] = i -> i == 0 ? 1 : i * fac[0].apply( i - 1);


            UnaryOperator<Integer> factorial = fac[0];


            System.out.println(factorial.apply(5));

        }

    }

## 메소드 레퍼런스

Method Reference는 이미 구현되어있는 메소드를 참조해 매개변수의 정보와 리턴타입을 알아내, 불필요한 매개변수를 제거하는것이 목적이다. 예를 들어 정수 값 두개를 받아 둘 중 큰 값을 리턴해주는 메소드를 람다식으로 표현하려한다.

    IntBinaryOperator op = (int x, int y) -> {
        if(x > y) return x;
        return y;

    };
    System.out.println(op.applyAsInt(3, 7)); // 7

앞서 설명한 함수적 인터페이스를 개발자가 직접 만들어 사용할 수도 있지만, 자바에서 제공하는 표준 인터페이스들을 사용해서 람다식을 구현할 수 있다. 상단의 코드에 있는 IntBinaryOperator는 BinaryOperator종류의 표준 함수 인터페이스이다. IntBinaryOperator는 두 개의 정수를 매개변수로 받아 정수값을 리턴해주는 추상메소드 applyAsInt()를 갖고있다. 람다식은 두 개의 정수를 매개변수로 받아 둘 중 큰 값을 반환해준다. 기존에 Math 클래스의 max()를 사용해도 같은 결과를 얻을 수 있다.


    IntBinaryOperator op1 = (int x, int y) -> {return Math.max(x, y);}; // 앞서 배운 것을 모두 적용할 경우

    IntBinaryOperator op2 = (x, y) -> Math.max(x, y);

    System.out.println(op2.applyAsInt(3, 7)); // 7

람다식은 이미 구현되어 있는 Math.max()를 참조하여 사용하고 있다. 이럴 때 굳이 매개변수 (x,y)를 명시적으로 표현하지 않고 사용할 수 있다.

    IntBinaryOperator op = Math::max;

    System.out.println(op.applyAsInt(3, 7)); // 7

### 3가지 메소드 레퍼런스
1. 정적 메소드, 인스턴스 메소드 참조

전 코드의 max()메소드는 Math클래스의 정적 메소드이기에 정적(static)메소드 참조방식이 된다.      
만약 인스턴스 메소드일 경우 인스턴스를 생성한 후에 인스턴스::메소드명으로 사용할 수 있다.

2. 매개변수의 메소드 참조 방식

이번에는 매개변수로 받은 인자의 메소드를 참조하는 방식이다. Integer클래스에 있는 compareTo()를 사용해 보겠다. compareTo()는 매개변수로 들어온 인자가 호출한 인자보다 값이 큰 경우 -1, 같은 경우 0, 작은 경우 1을 리턴한다.      
ToIntBiFunction은 Function 인터페이스 중 하나로써. 두 개의 매개변수를 받아 람다식의 로직을 사용하여 applyAsInt()를 사용했을 때 int 타입을 반환해주는 함수적 인터페이스이다.

    class Driver {
        public static void main(String[] args) {
            Integer x = 3;
            Integer y = 7;
            ToIntBiFunction<Integer, Integer> func = Integer::compareTo; // (x, y) -> x.compareTo(y);

            System.out.println(func.applyAsInt(x, y)); // -1
        }
    }

3. 생성자 참조 방식

생성자 또한 일종의 메소드이기 때문에 메소드 레퍼런스로 쓸 수 있다.

    class Person {
        private String name;
        private Integer age;
        public Person(String name) {
            this.name = name;
            this.age = 0;
        }
        public Person(String name, Integer age) {
            this.name = name; this.age = age;
        }
        @Override
        public String toString() {
             return "[" + this.name + " : " + this.age + "]";
        }
    }

    class Driver {
        public static void main(String[] args) {
            Function<String, Person> func1 = Person::new;
            Function<String, Integer, Person> func2 = Person::new; // 넘어오는 매개변수의 갯수로 어떤 생성자를 호출할 지 찾아줌
            System.out.println(func1.apply("Dom")); // [Dom : 0]
            System.out.println(func2.apply("Dom", 28)); // [Dom : 28]
        }
    }

4. 활용 예시

람다식을 만들 수 있는 타겟타입도 변수가 될 수있다. 람다식과 같이 활용하면 메소드도 매개변수처럼 사용할 수 있다.(정확히는 메소드를 구현한 함수적 인터페이스를 변수로 사용한 것)

Calculator

    @FuntionalInterface

    interface Calculator {
        int calc(int n);
    }

Driver

    class Driver {
        public static void main(String[] args) {
            int n = 2; // int n과 구별하기 위해 람다식에는 v라는 변수명을 사용.
            printCalc(n, v -> v + 1); // n + 1, 출력값 3
            printCalc(n, v -> v - 1); // n - 1, 출력값 1
            printCalc(n, v -> v * 2); // n * 2, 출력값 4
            }
            public static void printCalc(int n, Calculator cal) {
                System.out.println(cal.calc(n));
        }
    }

## 참고  
* https://juyoung-1008.tistory.com/48
* https://sehun-kim.github.io/sehun/java-lambda-stream/
* https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html
* https://stackoverrun.com/ko/q/6924977
* https://www.oracle.com/technical-resources/articles/java/architect-lambdas-part1.html