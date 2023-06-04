## 아이템36. 비트 필드 대신 EnumSet을 사용하라

#### 문제의 코드
```java
public class Text{
  public static final int STYLE_BOLD = 1 << 0; // 1
  public static final int STYLE_ITALIC = 1 << 1; // 2
  public static final int STYLE_UNDERLINE = 1 << 2; // 4
  public static final int STYLE_STRIKETHROUGH = 1 << 3; // 8
  
  public void applyStyles(int styles) { ... }
```

다음과 같은 식으로 비트별 OR을 사용해 여러 상수를 하나의 집합으로 모을 수 있으며, 이렇게 만들어진 집합을 비트 필드라고 한다
```java
text.applyStyles(STYLE_BOLD | STYLE_ITALIC);
```

비트 필드를 사용하면 비트별 연산을 사용해 합집합과 교집합 같은 집합 연산을 효율적으로 수행할 수 있다.

#### 취약점
- 비트 필드 값이 그대로 출력되면 단순한 정수 열거 상수를 출력할 때보다 해석하기가 훨씬 어렵다.
- 비트 필드 하나에 녹아 있는 모든 원소를 순회하기 까다롭다.
- 최대 몇 비트가 필요한지를 API 작성 시 미리 예측하여 적절한 타입(보통은 int나 long)을 선택해야 한다.


#### 해결법
- Set 인터페이스를 구현한 EnumSet 클래스를 사용하면 된다.(타입 안전)
- EnumSet의 유일한 단점은 불변 EnumSet을 만들 수 없다.(java9까지) 향후 릴리스될때까지는 Collections.unmodifiableSet으로 EnumSet을 감싸 사용할 수 있다.

#### 해결코드
```java
public class Text{
  public enum Style { BOLD, ITALIC, UNDERLINE, STRIKETHROUGH }
  
  public void applyStyles(Set<Style> styles) { ... }
```

다음은 applyStyles 메서드에 EnumSet 인스턴스를 건네는 클라이언트 코드다.
```java
text.applyStyles(EnumSet.of(Style.BOLD, Style.ITALIC));
```
