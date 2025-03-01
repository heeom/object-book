# Ch02. 객체지향 프로그래밍

## 객체지향 프로그래밍으로의 전환

- 어떤 객체들이 필요한지를 먼저 고민.
- 객체를 공동체의 일원으로 보고, 그들의 협력을 먼저 설계하자.
- 클래스는 그 이후이다.

## 클래스 구현

- 클래스는 외부와 내부로 구분.
- 어떤 걸 노출하고 감출지 결정하는 것이 핵심.
- 경계를 명확하게 잘 나눠야 `자율적인 객체`를 만들 수 있음.
  - 상태와 행동을 가지고 있어야 스스로 책임을 잘 수행할 수 있음.
  - 재사용성 + 확장하기 좋은 구조가 됨.

## 영화 예매 시스템

![영화 예매 시스템](https://github.com/user-attachments/assets/2c3728d5-6091-4fbb-9864-54e684a468ad)

- `돈`처럼 숫자로 나타낼 수 있는 데이터도 객체로 표현하면 의미 전달이 더 효과적이다.
  - 설계의 명확성 + 유연성 향상

```java
// as-is
BigDecimal money = 0;

// to-be
public class Money {
  private BigDecimal amount;

  Money(BigDecimal amount) {
    this.amount = amount;
  }
  // ...
}
```

- 객체는 다른 객체의 공개된 인터페이스를 통해 행동을 수행하도록 요청을 보냄.

  - 요청 받은 객체는 알아서 내부에서 해당 요청을 처리하고 응답한다.
    -> 클래스의 외부와 내부(public/private)를 나누는 경계 개념
    -> **관심사의 분리**라고도 볼 수 있음 - 요청한 객체는 요청 받은 객체가 어떻게 처리하든 관심 없음.

- 추상 클래스: 일부 공통된 동작이 있고, 세부 구현이 조금 다른 상황에서 유용하다.

  - 세부 구현을 자식 클래스에게 위임하는 `템플릿 메소드 패턴`을 사용하여 중복을 줄이고 상황에 따라 다르게 동작하도록 할 수 있다.

- 생성자를 통해서 `할인 조건`, `할인 정책`의 제약을 강제할 수 있다.

### 상속과 다형성

1. 의존성

- 코드의 의존성 != 실행 시점의 의존성
  - 다를수록 코드 이해 `어려워짐`
  - 다를수록 설계는 `더 유연, 더 확장하기 좋아짐`
    -> 트레이드 오프: 가독성 vs 유연성

```java
// 코드: DiscountPolicy에 의존
// 실행 시점: DiscountPolicy의 구체 클래스에 의존
public class Movie {
  public Movie(/* ... */ DiscountPolicy discountPolicy) {
    this.discountPolity = discountPolicy;
  }
}
```

2. 상속

- 부모 클래스의 모든 인터페이스를 물려받음
- 그렇기에 자식 클래스는 부모 클래스를 대신할 수 있음: 업캐스팅
  -> (SOLID의 L, 리스코프 치환 법칙과 연관)

3. 다형성

- 메시지 != 메소드
  - 추상 클래스 DiscountPolicy의 `calculateDiscountAmount`는 메시지 (개념 or 추상적 존재)
  - 실제로 실행되는 구체 클래스 `AmountDiscountPolicy` or `PercentDiscountPolicy`의 메소드 (구체적 존재)
  - 실행되는 메소드는 메시지를 수신하는 객체에 따라 달라짐. -> 다형성

## 추상화와 유연성

### 추상화의 장점

1. 요구사항을 고수준에서 표현 가능
   -> 세부적인 내용을 무시하고 더 큰 틀에서 표현할 수 있기 때문
2. 설계가 유연해짐
   -> 기존 구조를 유지한 채로 추가 및 확장 가능.

### 유연한 설계

- 할인 정책이 있는 경우 `DiscountPolicy`를 상속받은 자식 클래스들이 책임을 가진다.
- 할인 정책이 없는 경우에는 아래와 같이 구현할 수 있다.

```diff
// Movie class
// 👎
public Money calculateMovieFee(Screening screening) {
+  if(discountPolicy == null) {
+    return fee
+  }
  return fee.minus(discoutPolicy.calculateDiscountAmount(screening));
}
```

```java
// Movie 클래스는 그대로. 자식 클래스 하나 추가.
// 👍
public class NoneDiscountPolicy extends DiscountPolicy {

  @Override
  protected Money getDiscountAmount(Screening screening) {
    return Money.ZERO;
  }
}
```

> 첫번째 예시는 책임의 위치가 Movie 클래스로 변경되면서 일관성 깨짐.
> 두번째 예시는 책임의 위치를 DiscountPolicy 계층에 유지시킴.

### 추상 클래스와 인터페이스 트레이드오프

- 구현과 관련된 모든 것들이 트레이드오프의 대상.
- 사소한 결정이라도 충분한 고민을 통해 결론을 내자.

### 코드 재사용

- 합성: 다른 객체의 인스턴스를 자신의 필드로 포함시켜 재사용하는 방법

### 상속

- 설계에 안 좋은 영향을 미침.
  1. 캡슐화 위반
  - 부모 클래스의 구현이 자식 클래스에게 노출되기 쉬움.
  - 자식 클래스와 부모 클래스의 결합도를 높인다.
  2. 설계를 유연하지 못하게 만듦.
  - 컴파일 시점에 결정됨. 실행 시점에는 객체 종류 변경 불가.

### 합성

- 상속은 컴파일 시점에 부모 클래스와 자식 클래스를 강하게 결합시킴.
- 합성은 인터페이스를 통해 약하게 결합.
- 합성: 인터페이스에 정의된 메시지를 통해서만 코드를 재사용하는 방법
  - 상속의 단점을 모두 해결.
    1. 인터페이스에 정의된 메시지로만 재사용 가능 -> 캡슐화 깨지않음.
    2. 메시지를 통해서 느슨하게 결합 -> 의존하는 인스턴스 교체가 쉬움. (유연)
- 상속을 아예 쓰지 말라는 것이 아님. 둘을 병행해서 사용해야함.
