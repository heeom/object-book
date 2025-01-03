## 협력과 메시지

### 클라이언트-서버모델

- 메세지를 매개로하는 요청과 응답의 조합이 두 객체 사이의 협력을 구성한다.

### 메시지와 메시지 전송

- 메시지 전송
    - 한 객체가 다른 객체에게 도움을 요청하는 것
    - 메시지 수신자, 오퍼레이션 명, 인자(argument)의 조합
    - `condition.isSatisfiedBy(screening)`
- 메시지와 메서드
    - 메시지를 수신했을때 수행되는 코드는 구현체에 따라 달라진다.
- public interface와 오퍼레이션
    - 객체가 의사소통을 위해 외부에 공개하는 메시지의 집합
    - 오퍼레이션 = 어떤 행동에 대한 추상화
    - 메서드 = 오퍼레이션에 대한 실제 구현

## 인터페이스와 설계품질

- 좋은 인터페이스는 최소한의 인터페이스와 추상적인 인터페이스
    - 메세지를 먼저 선택함으로써 협력과는 무관한 오퍼레이션이 인터페이스에 스며드는 것을 방지한다.
    - 메시지가 객체를 선택할 수 있게 한다.

### 퍼블릭 인터페이스의 품질에 영향을 미치는 원칙

- 디미터 법칙
    - 협력 경로를 제한하여 결합도를 낮출 수 있다.
    - this 객체, this의 속성, 메서드의 매개변수, 메서드 내에서 생성된 지역 객체
    - 디미터 법칙을 따르면 메시지 수신자의 내부구조가 전송자에게 노출되지 않으며, 메시지 전송자는 수신자의 내부 구현에 결합되지 않으므로 클라이언트와 서버 사이에 낮은 결합도를 유지할 수 있다.
- 묻지말고 시켜라
    - 상태를 묻는 오퍼레이션 → 행동을 요청하는 오퍼레시션으로 대체
- 의도를 드러내는 인터페이스
    - 메서드 네이밍 방법
        - 메서드가 작업을 어떻게 수행하는지 나타내도록 네이밍하는 방법
            - 메서드에 대해 제대로 커뮤니케이션 하지 못하고, 메서드 수준에서 캡슐화를 위반한다
        - 어떻게가 아니라 무엇을 하는지를 드러내는 방법(의도를 드러내는 선택자)
            - 코드를 읽고 이해하기 쉽게 하고 유연한 코드를 만든다
                - 무엇을 하느냐에 초점을 맞추면 클라이언트의 관점에서 동일한 작업을 수행하는 메서드들을 하나의 타입계층으로 묶을 수 있는 가능성이 커지고 → 다양한 타입의 객체가 참여할 수 있는 유연한 협력을 얻게 된다.
            - 어떻게 수행하는지 드러내는 이름 → 메서드 내부 구현을 설명하는 이름
            - 오퍼레이션은 클라이언트가 객체에게 무엇을 원하는 지를 표현해야한다. 즉 객체 자신이 아닌 클라이언트의 의도를 표현하는 이름을 가져야한다.

## 원칙의 함정

- 디미터 법칙은 결합도와 관련된 것
    - 결합도가 문제가 되는 것은 객체의 내부구조가 외부로 노출되는 경우
    - 기차 충돌처럼 보이는 코드라도 객체의 내부 구현에 대한 어떤 정보도 외부로 노출하지 않는다면 디미터 법칙을 준수한것
- 결합도와 응집도의 충돌
    - 무조건 묻지말고 시켜라, 디미터 법칙을 준수한다고 위임메서드를 추가한다면 객체는 자신과 상관없는 책임을 떠안게 될 수도 있으므로 응집도가 낮아지고 다른 객체와 강하게 결합하게 되는 결과를 낳을 수도 있다.

## 명령-쿼리 분리 원칙

- 명령과 쿼리를 엄격하게 분류하면 객체의 부수효과를 제어하기 쉬워진다.
    - 부수효과를 가지는 명령으로부터 부수효과를 가지지 않는 쿼리를 분리함으로써 제한적으로 참조투명성의 혜택을 볼 수 있다.
- 명령과 쿼리를 분리하기 위한 규칙
    - 객체의 상태를 변경하는 명령은 반환값을 가질 수 없다.
    - 객체의 정보를 반환하는 쿼리는 상태를 변경할 수 없다.