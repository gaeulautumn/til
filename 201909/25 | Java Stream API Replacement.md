## Java Stream API Replacement


리스트 내 필드의 합을 구하기 위해 stream API 를 사용했다.


```java
int transcnt = alioRcvPointList.stream().collect(Collectors.summingInt(AlioRcvPointVO::getTranscnt));
```

intellij 에서는 다음과 같은 warning 을 주며 불필요한 임시 객체 생성을 막을 수 있는 단순화된 방식을 알려주었다.

```java
int transcnt = alioRcvPointList.stream().mapToInt(AlioRcvPointVO::getTranscnt).sum();
```

무심코 쓰게되는 stream API 가 불필요한 연산을 불러올 수도 있다. intellij 에서 가이드해주는 다음 내용을 참고하면 좋을 것 같다.

```java
The 'collect(summingInt())' call can be replaced with 'mapToInt().sum()' less... (Ctrl+F1)
Inspection info: This inspection reports stream API call chains which can be simplified. It allows to avoid creating redundant temporary objects when traversing a collection.
The following call chains are replaced by this inspection:
collection.stream().forEach() → collection.forEach()
collection.stream().collect(toList/toSet/toCollection()) → new CollectionType<>(collection)
collection.stream().toArray() → collection.toArray()
Arrays.asList().stream() → Arrays.stream() or Stream.of()
IntStream.range(0, array.length).mapToObj(idx -> array[idx]) → Arrays.stream(array)
IntStream.range(0, list.size()).mapToObj(idx -> list.get(idx)) → list.stream()
Collections.singleton().stream() → Stream.of()
Collections.emptyList().stream() → Stream.empty()
stream.filter().findFirst().isPresent() → stream.anyMatch()
stream.collect(counting()) → stream.count()
stream.collect(maxBy()) → stream.max()
stream.collect(mapping()) → stream.map().collect()
stream.collect(reducing()) → stream.reduce()
stream.collect(summingInt()) → stream.mapToInt().sum()
stream.mapToObj(x -> x) → stream.boxed()
stream.map(x -> {...; return x;}) → stream.peek(x -> ...)
!stream.anyMatch() → stream.noneMatch()
!stream.anyMatch(x -> !(...)) → stream.allMatch()
stream.map().anyMatch(Boolean::booleanValue) -> stream.anyMatch()
IntStream.range(expr1, expr2).mapToObj(x -> array) -> Arrays.stream(array, expr1, expr2)
Collection.nCopies(count, ...) -> Stream.generate().limit(count)
stream.sorted(comparator).findFirst() -> Stream.min(comparator)
Note that the replacements semantic may have minor difference in some cases. For example, Collections.synchronizedList(...).stream().forEach() is not synchronized while Collections.synchronizedList(...).forEach() is synchronized. Or collect(Collectors.maxBy()) would return an empty Optional if the resulting element is null while Stream.max() will throw NullPointerException in this case.
```