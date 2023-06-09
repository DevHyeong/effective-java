# 아이템 26. 로 타입(raw type)은 사용하지 말라
- 로타입 : 예를 들어 List<E>의 로 타입은 List다.
- 로 타입을 쓰면 제네릭이 안겨주는 안정성과 표현력을 모두 잃게 된다.
- 아래 예는 컴파파일이 아닌 런타임 시에 오류가 발생한다.
  
변경전 
```
private final Collection stamps = ...;

stamps.add(new Coin(...)); // unchecked call 경고를 내뱉음

for(Iterator i = stamps.iterator(); i.hasNext(); ){
  Stamp stamp = (Stamp) i.next(); // ClassCastException 발생
  stamp.cancel();
}
```

변경후
```
private final Collection<Stamp> stamps = ...;
```
이렇게 선언하면 컴파일러는 stamps에는 Stamp의 인스턴스만 넣어야 함을 인지하게 된다. 이제는 런타임이 아닌 컴파일에서 오류가 발생하게 된다.
  
### 그렇다면 절대 써서는 안되는 로 타입을 만들어 놓은 이유는?
- 호환성 때문, (기존 코드를 모두 수용하면서 제네릭을 사용하는 새로운 코드와도 맞물려 돌아가게 해야하기 때문)
  
### 예외
- class 리터럴에는 로 타입을 사용해도 된다. 
  - ex) List.class, String[].class, int.class는 허용, List<String>.class, List<?>.class 허용하지 않음
- instanceof 연산자 사용시
  - 런타임에는 제네릭 타입 정보가 지워지므로 instanceof 연산자는 비한정적 와일드카드 타입 이외에 매개변수화 타입에는 적용할 수 없다.
  - 다음은 제네릭 타입에 instanceof를 사용하는 올바른 예다.
  ```
    if(o instanceof Set){ // 로타입
        Set<?> s = (Set<?>) o; // 와일드카드 타입
  ```

### 정리
- 로 타입을 사용하면 런타임에 예외가 일어날수 있으니 사용하면 안된다
- 로 타입은 제네릭이 도입되기 이전 코드와의 호환성을 위해 제공될 뿐이다.
- Set<Object>는 어떤 타입의 객체도 저장할 수 있는 매개변수화 타입이고, Set<?>는 모종의 타입 객체만 저장할 수 있는 와일드카드 타입이다.
- Set<Object>와 Set<?>는 안전하지만, 로 타입인 Set은 안전하지 않다.

