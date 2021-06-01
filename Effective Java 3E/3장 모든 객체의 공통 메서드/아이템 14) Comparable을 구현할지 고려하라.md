- Comparable 인터페이스의 compareTo 메서드를 알아보자.

- Object 의 equals 와 같은 비교에서 차이점이 있는데 compareTo 는 단순 동치성 비교에 더해 순서까지 비교할 수 있고 제네릭하다.

- Comparable 을 구현했다는 것은 그 클래스의 인스턴스들에는 자연적인 순서(natural order)가 있음을 뜻한다. 그래서 Comparable 을 구현한 객체들의 배열은 다음처런 손쉽게 정렬할 수 있다.

```java
Arrays.sort(a);
```

- 검색, 극단값 계산, 자동 정렬되는 컬렉션 관리도 역시 쉽게 할 수 있다. 다음 프로그램은 명령줄 인수들을 (중복은 제거하고) 알파벳순으로 출력한다. String이 Comparable 을 구현한 덕분이다.

```java
public class WordList {
    public static void main(String[] args) {
        Set<String> s = new TreeSet<>();
        Collections.addAll(s, args);  // args = [ b, c, a ];
        System.out.println(s);  // [ a, b, c ];
    }
}
```

- Comparable 을 구현하여 수많은 제네릭 알고리즘과 컬렉션의 힘을 누릴 수 있으니 알파벳, 숫자, 연대 같이 순서가 명확한 값 클래스를 작성한다면 반드시 Comparable 인터페이스를 구현하자.

```java
public interface Comprable<T> {
    int compareTo(T t);
}
```

- compareTo 메서드의 일반 규약은 equals 의 규약과 비슷하다.

> 이 객체와 주어진 객체의 순서를 비교한다. 이 객체가 주어진 객체보다 작으면 음의 정수를, 같으면 0을, 크면 양의 정수를 반화한다. 이 객체와 비교할 수 없는 타입의 객체가 주어지면 ClassCastException 을 던진다. <br><br>
> 다음 설명에서 sgn(표현식) 표기는 수학에서 말하는 부호 함수(signum function)를 뜻하며, 표현식의 값이 음수, 0, 양수일 때 -1, 0, 1 을 반환하도록 정의했다.<br><br>
>
> - Comparable을 구현한 클래스는 모든 x,y 에 대해 sgn(x.compareTo(y)) == -sgn(y.compareTo(x)) 여야 한다. x.compareTo(y) 는 y.compareTo(x) 가 예외를 던질 때에 한해 예외를 던져야 한다.<br><br>
> - Comparable을 구현한 클래스는 추이성을 보장해야 한다. 즉, (x.compareTo(y) > 0 && y.compareTo(z) > 0) 이면 x.compareTo(z) > 0 이다.<br><br>
> - Comparable 을 구현한 클래스는 모든 z에 대해 x.compareTo(y) == 0 이면 sgn(x.compareTo(z)) == sgn(y.compareTo(z)) 다.<br><br>
> - 이번 권고가 필수는 아니지만 꼭 지키는게 좋다. (x.compareTo(y) == 0) == (x.equals(y)) 여야 한다. Comparable 을 구현하고 이 권고를 지키지 않는 모든 클래스는 그 사실을 명시해야 한다. 다음과 같이 명시하면 적당하다.<br><br>
>   "주의: 이 클래스의 순서는 equals 메서드와 일관되지 않다."

- 모든 객체에 대해 전역 동치관계를 부여하는 equals 메서드와 달리, **compareTo 는 타입이 다른 객체를 신경 쓰지 않아도 된다.** 타입이 다른 객체가 주어지면 간단히 ClassCastException 을 던져도 되며, 대부분 그렇게 한다.

- hashCode 규약을 지키 못하면 해시를 사용하는 클래스와 어울리지 못하듯, compareTo 규약을 지키지 못하면 비교를 활용하는 클래스와 어울리지 못한다.

- 비교를 활용하는 클래스의 예로는 정렬된 컬렉션인 TreeSet 과 TreeMap, 검색과 정렬 알고리즘을 활용하는 유틸리티 클래스인 Collections 와 Arrays 가 있다.

- equals 와 비슷하게 동치성 검사도 반사성, 대칭성, 추이성을 충족해야 하며 주의사항도 똑같다. 기존 클래스를 확장한 구체 클래스에서 새로운 값 컴포넌트를 추가했다면 compareTo 규약을 지킬 방법이 없다. 객체 지향적 추상화의 이점을 포기할 생각이 아니라면 말이다.

- 우회법도 비슷한데 Comparable 을 구현한 클래스를 확장해 값 컴포넌트를 추가하고 싶다면, 확장하는 대신 독립된 클래스를 만들고, 이 클래스에 원래 클래스의 인스턴스를 가리키는 필드를 두자. (컴포지션) 그런 다음 내부 인스턴스를 반환하는 '뷰' 메서드를 제공하며 ㄴ된다.
