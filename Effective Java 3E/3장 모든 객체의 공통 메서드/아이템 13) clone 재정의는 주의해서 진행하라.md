- clone 메서드는 Object에 정의된 메서드로, 원본 객체의 필드값과 동일한 값을 가지는 새로운 객체를 생성한다. **복제** 하는 메서드 인데, clone 메서드를 사용하면 같은 필드를 가지고 있는 객체를 생성해 준다.

- 복제 라고 표현하고 있는데, 사실 개발을 하면서 비슷한 "복사" 라고 착각 하는 방법으로 얕은 복사를 이용했었다.

## 얕은 복사(Shallow Copy) vs 깊은 복사(Deep Copy)

**1. 얕은 복사(Shallow Copy)**

- 객체를 복사할 때, 해당 객체만 복사하여 새 객체를 생성한다.

- 복사된 객체의 인스턴스 변수는 원본 객체의 인스턴스 변수와 같은 메모리 주소를 참조한다.

- 따라서, 해당 메모리 주소의 값이 변경되면 원본 객체 및 복사 객체의 인스턴스 변수 값이 같이 변경 된다.

- 일반적인 객체 할당의 방법이 이러한 방법이다.

![캡처](https://user-images.githubusercontent.com/50399804/120071955-5eb95e00-c0cc-11eb-8330-6514583b4c5e.JPG)  
![캡처2](https://user-images.githubusercontent.com/50399804/120071957-5fea8b00-c0cc-11eb-9738-d23dbad472e4.JPG)

**2. 깊은 복사(Deep Copy)**

- 객체를 복사할 때, 해당 객체의 필드값과 동일한 값을 가지는 새로운 객체를 생성한다. 객체를 구현한 클래스에 따라 복사의 범위가 다를 수 있다.

- 얕은 복사와 달리 주소 값을 참조하는게 아닌 새 주소에 담으므로 원본 객체
  혹은 복사된 객체를 변경해도 값이 변경되지 않는다.

- Object의 [Clone](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#clone--) 메소드 사용이 이에 해당한다.

```java
@HotSpotIntrinsicCandidate
protected native Object clone() throws CloneNotSupportedException;
```

- Object의 clone 메소드는 접근 제어자가 protected 로 되어있어, 리플렉션을 쓰지 않는 이상은 오버라이딩한 메서드에 접근할 수 있다는 게 보장되지 않는다.

- Object Clone 메소드를 오버라이딩하여 사용하면 CloneNotSupportedException 이 발생하는데 이유는 다음과 같다.

```java
/* Thrown to indicate that the clone method in class Object has been called to clone an object,
 * but that the object's class does not implement the Cloneable interface.
 */
```

- Cloneable 인터페이스를 구현하여야 Object의 Clone 메소드를 사용할 수 있는 것이다. ([마커 인터페이스](http://wonwoo.ml/index.php/post/1389)로 작동)

- 일반적인 Clone 메소드의 규약은 다음과 같다.

1. x.clone() != x
2. x.clone().getClass() == x.getClass()
3. x.clone().equals(X)
4. x.clone().getClass() = x.getClass()
