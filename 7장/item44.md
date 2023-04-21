
## 아이템44. 표준 함수형 인터페이스를 사용하라
- [java.util.function 패키지](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)를 보면 다양한 용도의 표준 함수형 인터페이스가 담겨 있다.
- 필요한 용도에 맞는 게 있다면 직접 구현하지 말고 표준 함수형 인터페이스를 활용하라.
- 아래의 기본 인터페이스(5개)는 기본 타입인 int, long, double용으로 각 3개씩 변형이 생겨난다. (ex) IntPredicate, LongBinaryOperator, LongFunction<int[]> )
- 표준 함수형 인터페이스 대부분은 기본 타입만 지원한다. 그렇다고 기본 함수형 인터페이스에 박싱된 기본 타입을 넣어 사용하지는 말자 (특히 계산량이 많을 때는 성능이 느려진다)

### Operator 인터페이스
- 반한값과 인수의 타입이 같은 함수를 뜻한다.
- 인수가 1개인 UnaryOperator와 2개인 BinaryOperator로 나뉜다.

### Predicate 인터페이스
- 인수를 하나를 받아 boolean을 반환하는 함수


### Function 인터페이스
- 인수와 반환 타입이 다른 함수


### Supplier 인터페이스
- 인수를 받지 않고 값을 반환하는 함수


### Consumer 인터페이스
- 인수를 하나 받고 반환값은 없는(인수를 소비하는) 함수

```java
  
```


### Comparator<T> 인터페이스
- int compare(T o1, T o2); // 추상 메서드로 제공
- o1 < o2이면 음수, o1 == o2 이면 0, o1 > o2 이면 양수를 리턴

  
### @FunctionalInterface 
직접 만든 함수형 인터페이스에 사용되는 어노테이션, 다음과 같은 특징이 있다.
- 해당 클래스의 코드나 설명 문서를 읽을 이에게 그 인터페이스가 람다용으로 설계된 것임을 알려준다.
- 해당 인터페이스가 추상 메서드를 오직 하나만 가지고 있어야 컴파일되게 해준다.
- 그 결과 유지보수 과정에서 누군가 실수로 메서드를 추가하지 못하게 막아준다.  
