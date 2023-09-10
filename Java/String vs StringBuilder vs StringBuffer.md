# 목차
1. String
2. StringBuilder
3. StringBuffer
4. 결론
5. 출처

<br>

## 1. String
- `Immutable(불변)`의 특성이 있음
- `String` 클래스의 문자열을 저장하는 `char[]`을 보면 `final`로 선언되어 있음
``` java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. **/
    private final char value[];
    ...
}
```
- 따라서 한 번 할당한 문자열을 변경하는 것은 불가능하며, 더하기 연산을 통해 문자열을 이어줄 경우 새로운 객체가 생성되어 재할당 됨
``` java
String s = "hello";
System.out.println(s.hashCode());  // 99162322
s += " world";
System.out.println(s.hashCode());  // 1776255224
```
- 반복적으로 문자열을 이어 붙이면 `Heap` 영역에 참조를 잃은 문자열 객체가 계속 쌓이게 됨
  - 추후 GC에 의해 수거가 되지만, 메모리 관리 측면에서는 효율적인 코드로 볼 수 없음
  - 계속해서 객체를 생성하기 때문에 연산 속도 측면에서 비효율적
- 이러한 성능 개선을 위해 JDK1.5 이상부터 `String` 사용시, 컴파일 단계에서 `StringBuilder`가 사용됨

<br>

## 2. StringBuilder
- `mutable(가변)`의 특성이 있음
- 상속을 주는 부모 클래스 `AbstractStringBuilder` 클래스의 내부를 보면 `mutable`의 속성을 제공
``` java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /** The value is used for character storage **/
    char[] value;
    ...
}
```
- `append()` 메소드를 호출시, `char[]` 배열의 길이를 늘리고 같은 객체에 문자열을 더함
  - `StringBuilder` 객체에 변한이 없음
``` java
StringBuilder s = new StringBuilder("hello");
System.out.println(s.hashCode());  // 859417998
s.append("world");
System.out.println(s.hashCode());  // 859417998
```

<br>

## 3. StringBuffer 
- `Synchronized`가 적용되어 멀티스레드 환경에서 `Thread-safe`하게 동작
    - 쉽게 말해 동기화를 지원하는 `StringBuilder`
``` java
// StringBuffer 클래스의 append 메소드
@Override
public synchronized StringBuffer append(CharSequence s) {
    toStringCache = null;
    super.append(s);
    return this;
}
```

<br>

## 4. 결론 
### String
- 단순 문자열을 참조하거나 탐색, 검색이 잦을때 사용 권장
### StringBuilder
- 런타임 때, 반본적인 문자열 추가 연산이 많을때 사용 권장
- 단일 스레드 환경이라면 `StringBuffer`보다 성능이 좋음
### StringBuffer
- 멀티 스레드 환경에서 반복적인 문자 추가 연산이 많을때 사용 권장

<br>

## 5. 출처 
- [dnjscksdn98.log님의 [Java] String vs StringBuilder vs StringBuffer](https://velog.io/@dnjscksdn98/Java-String-vs-StringBuilder-vs-StringBuffer)
