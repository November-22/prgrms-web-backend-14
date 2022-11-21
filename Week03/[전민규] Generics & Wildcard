# Generics & Wildcard

당신은 아래의 코드를 읽을 수 있는가?

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)
```

조금 어렵다면 이 글을 읽어보자 

## ****Generics****

다양한 타입의 객체들을 다루는 method, collection에 **컴파일 시점에 타입 체크**를 해주는 기능
*→ 컴파일 시점에 체크를 하므로 **객체 타입 안정성을 높이**고 **형변환의 번거로움이 줄어**든다*

### 사용하는 이유

```java
class Box {
  private Object item;
  public void setItem(Object item) { this.item = item; }
  public Object getItem() { return this.item; }
}
```

Box 클래스는 Object 타입의 item을 멤버 변수로 작성되어졌다.

Box 클래스의 item을 사용하려면 아래와 같이 형 변환을 해야 하는 번거로움이 있다. 
→*컴파일러는 item의 타입이 String인지 모르기 때문*

```java
Box myBox = new Box();
myBox.setItem("my item");
String myItem = (String) myBox.getItem(); // 형변환 필요
```

이러한 번거로움을 해결하기 위해 컴파일 시점에 타입을 체크할 수 있는 Generics가 도입이 되었다.

### Generics의 사용

- 선언
    
    ```java
    class Box<T,M> {
      private T item;
    	private M shape;
      public void setItem(T item) { this.item = item; }
      public T getItem() { return item; }
    }
    
    ```
    
    - T : 타입 변수
    - Box : 원시 타입
    - <T,M> : 여러개의 타입변수 예시
    - Type 제한 : `class Box<T **extends Fruit** > {};` → Fruit의 자식 클래스만 허용
- 사용

```java
public static void main(String[] args) {
  Box<String,Rectangle> myBox = new Box(); // T, M 대신 실제 타입 지정
  myBox .setItem("my item");
  String myItem = myBox .getItem();
}
```

### Generics의 한계

- Generics 타입의 배열 생성 불가
→ new 연산자 특성 때문 (컴파일 시점에 타입T가 정확해야함)
- 참조변수와 생성자에 대입 된 타입 일치
→ `Box<**Apple**> appleBox = new Box<**Banana**>();`  : 불가능
- static 메서드, 변수에는 사용 불가
→ static 메서드, 변수는 인스턴스 변수를 참조할 수 없기 때문.

매개변수에 `Box<Fruit> fruitBox` 를 받아야 하는 static method가 있다.

```java
class Juicer<T extends Fruit> {
// 단순 Juice를 만들어 주는 역할을 하는 class 이므로 static method로 사용한다.
    static Juice makeJuice(FruitBox<T> box) { //오류 : static method의 타입 매개변수 불가! 
        String tmp = "";
        for (Fruit f : box.getList()) tmp += f + " ";
        return new Juice(tmp);
    }
}
```

그렇다면 makeJuice를 오버로딩을 해서 사용해야 하는가?

```java
class Juicer{
    static Juice makeJuice(FruitBox<Banana> box) { 
        String tmp = "";
        for (Fruit f : box.getList()) tmp += f + " ";
        return new Juice(tmp);
    }
    static Juice makeJuice(FruitBox<Apple> box){
        String tmp = "";
        for (Fruit f : box.getList()) tmp += f + " ";
        return new Juice(tmp);
    }
}
```

→ Generics는 컴파일할 때만 사용하므로 위 두 method는 **method 중복 정의**에 해당됨.

## Wildcard

Generics은 static method에서 Generics 타입 매개변수를 사용 할 수 없다. 이를 해결하기 위해 Wildcard가 고안되었다. 

- <? extends T> : T와 자손들 가능
- <? super T> : T와 그 조상들 가능
- <?> : 제한 없음

```java
class Juicer{
    static Juice makeJuice(FruitBox<? extends  Fruit> box){ //와일드 카드사용
            String tmp = "";
            for (Fruit f : box.getList()) tmp += f + " ";
            return new Juice(tmp);
    }
}
```

## Generic method

Method의 선언부에 Generic 타입이 선언된 method이다.

```java
class FruitBox<T extends Fruit & Eatable> extends Box<T> {
    static <M> void sort(List<M> list, Comparator<? super M> c){} 
} 
```

*Generic 클래스에 정의된 타입 매개변수 T와 Generic Method에 정의된 타입 매개변수M은 전혀 별개다.*

static 메서드, 변수는 인스턴스 변수를 참조할 수 없기 때문에 static 메서드, 변수는 Generic을 사용 할수 없다고 했다. 그러나 Generic method를 사용하면 다르다.

```java
static <T extends Fruit> Juice makeJuice(FruitBox<T> box) {}
```

→ 매개변수에 선언할 T에 대한 자료형을 미리 선언하게 되므로 문제가 없어진다!

## 정리

글 처음에 본 `Collections.sort` 를 보자 이제는 해석이 가능해 질 것이다.

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)
```

`<T extends Comparable<? super T>` 는 Generic Method임을 명시한다.

`extends Comparable<? super T>` 는 T 의 조상 클래스를 허용하는 wildcard 이다.

즉, List<T> 타입의 인스턴스( ex:List<Integer>)를 미리 생성하고 sort 매개변수에 넣어 호출하게 된다. 

### References

• [Java의 정석 3rd Edition](https://www.coupang.com/vp/products/57799011?itemId=200423111&vendorItemId=3137398432&src=1042503&spec=10304984&addtag=400&ctag=57799011&lptag=10304984I200423111V3137398432&itime=20221108130201&pageType=PRODUCT&pageValue=57799011&wPcid=16678801217588140404094&wRef=&wTime=20221108130201&redirect=landing&gclid=CjwKCAiA9qKbBhAzEiwAS4yeDS5edw8W0cibrkRs8QOEYBRxybehcCzLAsfEZHCUu3QPGT-bR_YNPhoC7AsQAvD_BwE&campaignid=18394378295&adgroupid=&isAddedCart=)
