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

- equals를 재정의한 클래스 모두에서 hashCode도 재정의해야한다. 그렇지 않으면 hashCode 일반 규약을 어기게 되어 해당 클래스의 인스턴스를 HashMap이나 HashSet 같은 컬렉션의 원소로 사용할 때 문제를 일으킨다.
- hashCode에 대한 Object 명세 (https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html)
  - equals 비교에 사용되는 필드정보가 변경되지 않았다면 그 객체의 hashCode 메서드는 몇번을 호출해도 항상 같은 값을 반환한다(애플리케이션 재실행은 제외)
  - equals가 두 객체를 같다고 판단했다면 두 객체의 hashCode는 똑같은 값을 반환해야한다.
  - equals가 두 객체를 다르다고 판단했더라도 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단 다른 객체에 대해서는 다른 값을 반환해야 해시테이블 성능이 좋아진다.

