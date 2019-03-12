### Flyweight(享元模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/flyweight1.png)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/flyweight2.png)

#### 意图

共享支持大量细粒度的对象

#### 使用场景
- 需要大量的对象，并且这些对象的外部状态是可以分离的
- 删除外部状态后，这些对象可以用很少的一些对象取代

#### 优缺点
- 由FlyweightFactory来实现Flyweight对象的共享，减少实例的创建，节约空间
- 通过共享来减少内部状态的消耗，通过计算来计算外部状态

#### 想法
Flyweight模式主要关键点是实现内部状态和外部状态的分离，通过删除外部状态实现对象的共享，同时通过Factory来管理共享对象的创建和引用。当需要使用时，只需要用共享的对象和通过存储或者计算得到的外部状态结合即可获取具有所需要特征的对象。

#### 示例代码
不同品牌为衣服的内部状态，颜色为衣服的外部状态，通过store来获取共享的不同品牌衣服
```
public interface Clothes {
  void show(String state);
}

public class Shirt implements Clothes {
  private String brand ;

  public Shirt(String brand) {
    this.brand = brand;
  }

  public String getBrand() {
    return brand;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o)
      return true;
    if (!(o instanceof Shirt))
      return false;

    Shirt shirt = (Shirt) o;

    return brand != null ? brand.equals(shirt.brand) : shirt.brand == null;
  }

  @Override
  public int hashCode() {
    return brand != null ? brand.hashCode() : 0;
  }

  @Override
  public void show(String state) {
    System.out.println("This is " + brand + " shirt.");
    System.out.println("This is " + state + " shirt.");
  }
}

public enum ClothesStore {
  INSTANCE;

  private Map<String,Clothes> map;

  ClothesStore() {
    map = new HashMap<>();
  }

  public Clothes getShirt(String brand) {
    map.putIfAbsent(brand, new Shirt(brand));
    return map.get(brand);
  }
}

public class Person {

  public static void main(String[] args) {
    Clothes nikeClothes = ClothesStore.INSTANCE.getShirt("nike");
    Clothes adidasClothes = ClothesStore.INSTANCE.getShirt("adidas");
    nikeClothes.show("red");
    nikeClothes.show("blue");
    adidasClothes.show("green");
    adidasClothes.show("black");
  }
}


```
