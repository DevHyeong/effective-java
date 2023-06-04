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

