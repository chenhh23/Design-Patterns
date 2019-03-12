### Decorator(装饰模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/decorator.png)

#### 意图

动态给对象添加额外的职责

#### 使用场景
- 希望动态给对象添加额外的职责，这些职责可以任意的组合
- 希望给某个对象而不是整个类添加职责，并不影响其他对象，这些组合是动态灵活的，而不是静态固定的
- 类定义不能被继承，或者被隐藏，或者组合太多，用继承实现功能的拓展导致子类数量太多

#### 优缺点
- 动态灵活，可以自由的任意的添加删除额外的职责
- 可以从简单的组件组合出复杂的操作，避免了层次结构中高层复杂特征的类
- 组合顺序不一样会导致很多相似的对象，难以维护，学习和排错

#### 想法
Decorator模式主要点是动态的给对象添加额外的职责，并且能够自由的对这些职责进行组合和删除，而不影响其他对象。避免了由于继承导致的类数量太对，类特征太复杂的情况。但是由于组合顺序不一样就会导致相似的对象，难以进行学习和排错。

#### 示例代码
构建一条上衣 品牌、颜色、价格信息
```
public interface Shirt {
  void show();
}

public class NikeShirt implements Shirt {
  @Override
  public void show() {
    System.out.println("This is nike shirt.");
  }
}

public abstract class ShirtDecorator implements Shirt {
  protected Shirt shirt;

  public ShirtDecorator(Shirt shirt) {
    this.shirt = shirt;
  }
}

public class RedColorDecorator extends ShirtDecorator {

  public RedColorDecorator(Shirt shirt) {
    super(shirt);
  }

  @Override
  public void show() {
    this.shirt.show();
    System.out.println("This is red shirt.");
  }

}

public class PriceDecorator extends ShirtDecorator  {
  private int price ;

  public PriceDecorator(Shirt shirt, int price) {
    super(shirt);
    this.price = price;
  }

  @Override
  public void show() {
    this.shirt.show();
    System.out.println(String.format("This shirt costs %d $.",price));
  }
}

public class Person {

  void print(Shirt shirt) {
    shirt.show();
  }

  public static void main(String[] args) {
    Person person = new Person();
    Shirt nikeShirt = new NikeShirt();
    Shirt shirtDecorator = new PriceDecorator(new RedColorDecorator(nikeShirt),50);
    person.print(shirtDecorator);
  }
}

```

