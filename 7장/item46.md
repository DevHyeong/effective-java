
## 아이템 46. 스트림에서는 부작용 없는 함수를 사용하라  

#### 냄새나는 코드
- 아래는 텍스트 파일에서 단어별 수를 세어 빈도표로 만드는 일을 한다.
```
  Map<String, Long> freq = new HashMap<>();
  try(Stream<String> words = new Scanner(file).tokens()){
    words.forEach(word -> {
      freq.merge(word.toLowerCase(), 1L, Long::sum);
    });
  }
```  
  
#### 리팩토링
```
  Map<String, Long> freq;
  try(Stream<String> words = new Scanner(file).tokens()){
    freq = words.collect(groupingBy(String::toLowerCase, counting()));
  }
```  

#### 피드백
- forEach 연산은 스트림 계산 결과를 보고할 때만 사용하고, 계산하는데는 사용하지말자.
- java.util.stream.Collectors 클래스의 groupingBy를 활용한 코드 간소화  
  
### Collectors 클래스
- 스트림을 사용하려면 꼭 배워야하는 개념
- 몇개의 메서드만 소개하겠다.
  
#### comparing 메서드
- 키 추출 함수를 받는 비교자 생성 메서드  
  
#### toMap 메서드
  
  
#### groupingBy 메서드 
  
