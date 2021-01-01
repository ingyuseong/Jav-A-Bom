# 접근 제어자(Access Modifier)

## 용도/목적

>개체는 자신의 상태를 스스로 책임져야 한다!

즉, **개체의 상태를 변경하는 주체는 개체 자신인 것이 이상적**이다. 따라서, 개체 외부에서 개체의 상태에 직접 접근하지 못하게 해야한다. 이때 바로 접근 제어자를 사용한다.

## 종류
`private`, `protected`, `public`, `default`(package)가 있다. 각각의 키워드가 허용하는 접근의 수준은 다음과 같다.

| Keyword     | 접근 허용 수준 |
| --- | --- |
| `private`   | class |
| `default`   | within its package |
| `protected` | derived classes and/or within same package |
| `public`    | everybody |

즉, 위에서부터 순서대로 클래스 내부 / 동일 패키지 / 동일 패키지 + 상속 받은 클래스 / 전체 수준이다.

이때, 헷갈릴 수 있는 부분은 **`private` 키워드의 경우 같은 instance가 아니라 같은 class의 경우 접근 가능**하다.

## 접근 제어자의 사용

### 보편적인 사용
접근 제어자는 보편적으로 다음과 같이 사용된다.
* 멤버 변수: `private` or `protected`
* 메서드: `public`

보편적으로 이렇게 사용되는 이유는 **메서드를 통해서만 멤버 변수에 접근할 수 있도록 하는 것이 바람직**하기 때문이다.

### `private` 메서드
보편적으로 메서드는 `public`을 사용한다고 했지만, `private` 메서드도 빈번하게 사용된다. **클래스 내에서 중복되는 코드를 최소화(DRY code)하기 `private` 메서드를 사용**한다.

생성자도 일반 메서드와 마찬가지로 똑같이 접근 제어자에 의해 접근의 수준이 제한된다. 따라서, 최소한 하나이상의 생성자는 `public`인 경우가 대부분이다. (`private` 생성자를 사용하는 경우는 다음 질문을 참고. [What is the use of making constructor private in class?](https://stackoverflow.com/questions/2062560/what-is-the-use-of-making-constructor-private-in-a-class))

## `default`
같은 패키지에 포함되어 있는 클래스끼리 서로 접근 가능.
* 같은 패키지: `public`처럼 작동
* 다른 패키지: `private`처럼 작동

보편적으로 다음과 같은 경우 사용된다.
* `public` 대신 패키지 접근 제어자를 사용할 수 있는 경우(package 내에서만 사용됨)
* `public`이 아닌 내포 클래스를 최상위 클래스로 분리하는 경우

### 클래스는 왜 `public`와 `default`만 사용가능한가?
당연한 것을 물어보는 질문이고, 생각해보면 바로 답을 알 수 있다. `private`의 경우는 그냥 말이 안된다. `private`의 허용 접근 수준을 고려할 때, 어불성설이다. `protected`의 경우도 말이 안되지만, 억지로 생각해봐도 접근 제어자와 무관하게 자식 클래스는 부모 클래스에 접근 가능하기 때문에 필요가 없다.

## References
* [Wikipedia Access Modifier](https://en.wikipedia.org/wiki/Access_modifiers)
* [What is the use of making constructor private in class?](https://stackoverflow.com/questions/2062560/what-is-the-use-of-making-constructor-private-in-a-class)
* [POCU Academy OOP & OOAD](https://pocu.academy/ko/Courses/COMP2500)