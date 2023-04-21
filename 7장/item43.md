
## 아이템43. 람다보다는 메서드 참조를 사용하라
- 람다가 익명 클래스보다 나은 점 중에서 가장 큰 특징은 간결함이다. 하지만 자바에서는 함수 객체를 람다보다 더 간결하게 만드는 방법이 있으니, 바로 메서드 참조다.
- 다음 코드는 키가 맵안에 없다면 키와 숫자 1을 매핑하고, 이미 있다면 기존 매핑 값을 증가시킨다.
  ```java
  map.merge(key, 1, (count, incr) -> count + incr);
  ```
  
- 자바 8에서 추가된 map의 merge 메서드
  ```java
   default V merge(K key, V value,
            BiFunction<? super V, ? super V, ? extends V> remappingFunction) {
        Objects.requireNonNull(remappingFunction);
        Objects.requireNonNull(value);
        V oldValue = get(key);
        V newValue = (oldValue == null) ? value :
                   remappingFunction.apply(oldValue, value);
        if (newValue == null) {
            remove(key);
        } else {
            put(key, newValue);
        }
        return newValue;
    }
  ```

- 아래 코드는 람다 대신 메서드 참조를 전달한 코드이다.
  ```java
  map.merge(key, 1, Integer::sum);
  ```
