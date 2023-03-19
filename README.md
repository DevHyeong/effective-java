# java-test

해당 저장소는 이펙티브 자바를 읽으면서 테스트를 해보고 싶은 자바 코드를 작성한 공간입니다.

이펙티브 자바 : http://www.yes24.com/Product/Goods/65551284



## 제 3장
### 아이템 10. equals는 일반 규약을 지켜 재정의하라

- equals 메서드는 논리적 동치성을 확인해야 할 때 재정의한다.
- equals 메서드는 동치관계를 구현하며 Object 명세에 적힌 규약을 따라야 한다. (https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html)
  - 반사성
  - 대칭성
  - 추이성 
  - 일관성
  - null-아님
- 구현방법
  - == 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.
  - instanceof 연산자로 입력이 올바른 타입인지 확인한다. ex. obj instanceof Student
  - 입력을 올바른 타입으로 형변환한다. ex. Student s1 = (Student) obj; 
  - 입력 객체와 자기 자신의 대응되는 핵심 필드들이 모두 일치하는지 하나씩 검사한다.
    - float와 double을 제외한 기본 타입 필드는 == 연산자
    - 참조타입필드는 equals 메서드
    - float와 double 필드는 정적 메서드인 compare 메서드 활용(equals 메서드는 성능상 좋지 않다.)
    
### 아이템 11. equals를 재정의하려거든 hashCode도 재정의하라
