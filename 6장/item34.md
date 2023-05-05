## 아이템34. int 상수 대신 열거 타입을 사용하라

열거 타입
- 열거 타입은 클래스이며, 상수 하나당 자신의 인스턴스를 하나씩 만들어 public static final 필드로 공개한다.
- 열거 타입은 밖에서 접근할 수 있는 생성자를 제공하지 않으므로 사실상 final이다.
- 열거 타입 선언으로 만들어진 인스턴스들은 딱 하나씩만 존재한다.

#### 열거 타입은 컴파일타임 타입 안전성을 제공한다.
- 열거 타입을 매개변수로 받는 메서드를 선언했다면 건네받은 참조는 열거 타입의 값 중 하나임이 확실하다.

#### 열거 타입 상수 각각을 특정 데이터와 연결지으려면 생성자에서 데이터를 받아 인스턴스 필드에 저장하면 된다.
- 열거 타입은 근본적으로 불변이라 모든 필드는 final이어야 한다.
- 필드를 public으로 선언해도 되지만, private으로 두고 별도의 public 접근자 메서드를 두는게 낫다.

#### 열거 타입은 자신 안에 정의된 상수들의 값을 배열에 담아 반환하는 정적 메서드인 values를 제공한다.
- 값들은 선언된 순서대로 저장된다.

#### 널리 쓰이는 열거 타입은 톱레벨 클래스로 만들고, 특정 톱레벨 클래스에서만 쓰인다면 해당 클래스의 멤버 클래스로 만든다.

#### 추상 메서드를 선언하여 열거 타입의 상수별로 동작을 다르게 구현할 수 있다. (상수별 메서드 구현)
- 열거 타입 상수끼리 코드를 공유하기 어렵다.
- 
#### 열거 타입에는 상수 이름을 입력받아 그 이름에 해당하는 상수를 반환해주는 valueOf(String) 메서드가 자동 생성된다.
















다음의 코드는 직원의 시간당 기본 임금과 그날 일한 시간이 주어지면 일당을 계산해주는 메서드다. 주중에 오버타임이 발생하면 잔업수당이 주어지고, 주말에는 무조건 잔업수당이 주어진다.

```java
enum PayrollDay{
  MONDAY, TUSEDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
  
  private static final int MINS_PER_SHIFT = 8 * 60;

  int pay(int minutesWorked, int payRate){
    int basePay = minutesWokred * payRate;
    int overtimePay;
    switch(this){
      case SATURDAY: case SUNDAY: //주말
        overtimePay = basePay / 2;
        break;
      default: // 주중
        overtimePay = minutesWorked <= MINS_PER_SHIFT ? 0 : (minutesWokred - MINS_PER_SHIFT) * payRate / 2;
    }
    return basePay + overtimePay;
  }
}
  
```

#### 단점
- 휴가와 같은 새로운 값을 열거 타입에 추가하려면 그 값을 처리하는 case문을 추가해야 한다.

#### 문제해결
- 잔업수당 계산을 private 중첩 열거 타입인 PayType으로 옮기고, PayrollDay 열거 타입의 생성자에서 이 중 적당한 것을 선택한다.
- PayrollDay 열거 타입(전략 열거 타입)은 잔업수당 계산을 PayType 열거 타입에 위임하여, switch문이나 상수별 메서드 구현이 필요 없게 된다.


```java
enum PayrollDay{
  MONDAY(WEEKDAY), TUSEDAY(WEEKDAY), WEDNESDAY(WEEKDAY), THURSDAY(WEEKDAY), FRIDAY(WEEKDAY), 
  SATURDAY(WEEKEND), SUNDAY(WEEKEND);
  
  private final PayType payType;
  
  PayrollDay(PayType payType){ this.payType = payType; }
  
  int pay(int minutesWokred, int payRate){
    return payType.pay(minutesWokred, payRate);
  }
  
  //전략 열거 타입
  enum PayType{
    WEEKDAY {
        int overtimePay(int minsWorked, int payRate){
          return minsWorked <= MINS_PER_SHIFT ? 0 : (minsWokred - MINS_PER_SHIFT) * payRate / 2;
        }
    },
    WEEKEND {
        int overtimePay(int minsWorked, int payRate){
          return minsWorked * payRate / 2;
        }    
    };
    
    abstract int overtimePay(int mins, int payRate);
    private static final int MINS_PER_SHIFT = 8 * 60;
    
    int pay(int minsWorked, int payRate){
      int basePay = minsWorked + payRate;
      return basePay + overtimePay(minsWorked, payRate);
    }
  }
}
  
```



### 결론
- 열거 타입은 언제 사용하면 될까? 
  - 필요한 원소를 컴파일 타임에 다 알 수 있는 상수 집합이라면 항상 열거 타입을 사용하면 된다. 
