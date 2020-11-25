---
layout: post
title: "[Java] 람다식(Lambda Expression)"
date: 2020-11-05 11:57
image: lambda.png
tags: [java,람다식,lambda]
categories: study
---
## [함수적 프로그래밍]

- y = f(x) 형태의 함수로 구성된 프로그래밍 기법

대용량 데이터를 처리해야 하는 경우 객체를 만드는 시간이 걸리기 때문에 객체 지향 프로그래밍 기법 보다는 함수적 프로그래밍 기법을 사용한다.

따라서, **현대적 프로그래밍**은 **객체 지향 프로그래밍 + 함수적 프로그래밍**이라 할 수 있다.

---

## [Java의 람다식]

Java는 Java 8부터 람다식(Lambda Expression)을 지원하기 시작했다. 람다식은 함수적 프로그래밍 기법이라고 할 수 있으며, 익명 함수(anonymous function)을 생성하는 식이다. 

위의 y = f(x) 라는 함수가 람다식에선 **(타입 매개변수,...) → { 실행문; ... }** 형식으로 바뀌는데 f(x)의 **x가 타입 매개변수**가 되고, **y가 실행문**이 된다고 생각하면 된다. 

다음은 람다식의 장점과 단점을 나타낸다.

### 장점

1. **코드의 간결함**
2. **컬렉션 요소(대용량 데이터)를 필터링 혹은 매핑하여 쉽게 데이터의 집계가 가능함**
3. **병렬처리의 가능과 안정적인 확장성**

### 단점

1. **일정 수준을 넘어가면 가독성에 영향을 미침**
2. **익숙하지 않은 사람에게는 쉽지 않은 호출 방식**

---

## [람다식의 사용 조건]

람다식은 함수적 인터페이스인 경우에만 사용이 가능하다.

- 여기서 **함수적 인터페이스**란 인터페이스가 단 한개의 추상 메소드를 정의하고 있는 인터페이스를 말한다. 이는 두개의 추상 메소드를 가지고 있다면, 람다식을 사용할 수 없다는 말이다. 이때 **@FunctionalInterface** 어노테이션을 인터페이스에 사용하면 해당 인터페이스는 하나의 메소드만 정의 할 수 있으면 두개 이상의 메소드를 정의하면 인터페이스에서 에러를 발생시킨다.

인터페이스를 구현하는 방식은 여러가지가 있다.

1. **인터페이스를 작성하고 클래스로 구현해서 사용** 
2. **인터페이스를 익명 함수로 구현해서 사용** 
3. **람다식으로 사용**

하나씩 예제를 살펴보자.

---

## [사용 방법 및 예제]

MaxNumber라는 interface가 있다. 그리고 getMaxNumber(int x, int y) 라는 메소드를 정의해놓았다.

```
//MaxNumber Interface
public interface MaxNumber {
    int getMaxNumber(int x, int y);
}
```

### 👉🏻 인터페이스를 클래스로 구현해서 사용 하는 방법 👈🏻

```
//MaxNumber Interface 구현 클래스
public class MaxNumberImpl implements MaxNumber {
    @Override
    public int getMaxNumber(int x, int y) {
        return x >= y ? x : y;
    }
}
```

MaxNumber 인터페이스를 MaxNumberImpl이라는 클래스로 구현한 후 Main 클래스에서 MaxNumber를 생성한 후 getMaxNumber 메소드를 호출해준다.

```
public class Main {
    public static void main(String[] args) {
        //1. 인터페이스를 직접 클래스로 구현 후 메인 메소드에서 생성 후 호출
        MaxNumber maxNumber = new MaxNumberImpl();

        System.out.println(maxNumber.getMaxNumber(3,1));
    }
}
//출력 결과 : 3
```

호출 결과 3과 1중 큰 수인 3이 출력 됨을 확인 할 수 있다.

### 👉🏻 인터페이스를 익명 함수로 구현해서 사용하는 방법 👈🏻

```
public class Main {
    public static void main(String[] args) {
        //2. 익명함수로 메인 클래스 내에서 구현하여 호출 
        MaxNumber maxNumber = new MaxNumber() {
            @Override
            public int getMaxNumber(int x, int y) {
                return x >= y ? x : y;
            }
        };
        System.out.println(maxNumber.getMaxNumber(3,1));
    }
}
```

MaxNumber Interface를 직접 메인 메소드 내에서 구현하여 호출한 방식이다.

똑같이 출력 결과가 3임을 알 수 있다.

### 👉🏻 람다식을 이용하여 호출하는 방법 👈🏻

```
public class Main {
    public static void main(String[] args) {
        //3. 람다식을 이용하여 호출 방식
        MaxNumber maxNumber = (x, y) -> x >= y ? x : y;
        System.out.println(maxNumber.getMaxNumber(3,1));
    }
}
```

MaxNumber Interface를 람다식으로 호출하고 메소드를 람다식 내 코드블록에서 구현해주었다.

똑같이 출력 결과가 3임을 알 수 있다.

3가지 방식을 알아보았다. 당장 3가지 코드를 비교해보아도 1번과 2번의 방식보다는 3번 람다식으로 표현한 방식이 더 코드가 간결하고 효율적임을 알 수 있다.

---

## [람다식의 생략 기법]

1. 매개변수의 타입이 생략 가능하다. 
    - `MaxNumber maxNumber = (str) → {System.out.println(str);};`
2. 매개변수가 한개인 경우 소괄호 생략이 가능하다.
    - `MaxNumber maxNumber = str → {System.out.println(str);};`
3. 코드블록 내 실행하는 코드가 한줄인 경우 중괄호 생략이 가능하다.
    - `MaxNumber maxNumber = (str) → System.out.println(str);`
4. 코드블록 내 실행하는 코드가 return문만 있는 경우 중괄호와 return의 생략이 가능하다.
    - `MaxNumber maxNumber = (str) → { return String.valueOf(str);};`
    - `MaxNumber maxNumber = (str) → String.valueOf(str);`
5. 매개변수가 없는 경우 소괄호만 표시해준다.
    - `MaxNumber maxNumber = () → System.out.println("매개변수 없음");`

---

## [정리]

- 람다식을 호출하는 경우는 프로젝트나 코드를 구현할 때 거의 내가 사용하지 않았던 방식이다. 하지만, 인프런에서 강의를 김영한님의 스프링부트 입문 강의를 들으면서 람다식 표현을 처음 접하게 되었고, 람다식을 실무에서도 많이 쓰는 표현 방식이라고 말씀해주셨고 문법과 사용방식을 정리하는 시간을 가지게 되었다.
- 가독성이 떨어지지 않는 수준에서 람다식을 효율적으로 사용한다면 클린 코드를 작성하는 방법과 더불어 간결하게 사용할 수 있는 좋은 방식인 것 같다.