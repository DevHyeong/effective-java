## 아이템35. ordinal 메서드 대신 인스턴스 필드를 사용하라

ordinal 메서드 : 해당 상수가 열거 타입에서 몇 번째 위치인지를 반환하는 메서드

#### 문제의 코드
- 다음 코드는 합주단의 종류를 연주자가 1명인 솔로(solo)부터 10명인 디텍트(dectet)까지 정의한 열거타입
```java
public enum Ensemble{
  SOLO, DUET, TRIO, QUARTET, QUINTET,
  SEXTET, SEPTET, OCTET, NONET, DECTET;
    
  public int numberOfMusicians() { return ordinal() + 1; }
}
```
#### 취약점
- 상수 선언 순서를 바꾸는 순간 numberOfMusicians는 오작동
- 이미 사용 중인 정수와 값이 같은 상수는 추가할 방법이 없다. 예를 들어 8중주(octet) 상수가 이미 있으니 똑같은 8명이 연주하는 복4중주(double quartet)는 추가할 수 없다.
- 값을 중간에 비워둘 수 없다. 예를 들어 12명이 연주하는 3중 4중주를 추가한다고 했을 때, 위의 코드가 올바르게 동작하기 위해서는 11명이 연주하는 합주단의 종류를 넣어야 한다.

#### 해결법
- 열거 타입 상수에 연결된 값은 ordinal 메서드로 얻지 말고 인스턴스 필드에 저장하자.
- Enum의 API 문서에 보면 ordinal에 대해 "대부분 프로그래머는 이 메서드를 쓸 일이 없다. 이 메서드는 EnumSet과 EnumMap 같이 열거 타입 기반의 범용 자료구조에 쓸 목적으로 설계되었다" 라고 설명되어 있다.

#### 해결코드
```java



```
