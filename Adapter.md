### Adapter(适配器)

- 类模式
![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/adapter-class.png)

- 对象模式
![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/adapter-object.png)

#### 意图
将已有接口转换成期望的接口，使得接口不兼容的类一起工作

#### 使用场景
- 已有的类其接口不满足需求 
- 创建可以复用的工具类，一些不可预期的接口需要使用到工具类当中的方法

#### 优缺点
类适配器和对象适配器是属于互补的两种适配器
对于类适配器来说:
- 能够重写被适配的行为
- 被适配的对象是确定的，因此如果需要适配不同的子类，需要不同的适配器

对于对象适配器来说：
- 不能够重写被适配的行为，如果需要重写被适配的行为，则需要构造不同的Adaptee子类对象
- 被适配的对象是一个引用，因此如果需要适配不同的子类，只需要传入不同的指针即可。

#### 额外
类适配器可以当作双向适配器来使用，因为类适配器用继承实现了Adapter和Adaptee接口的兼容性，因此Adapter和Adaptee使用的地方可以用双向适配器来协调工作.

#### 想法
Adapter 主要分为类Adapter和对象Adapter, 其中类Adapter是通过继承来获取Adaptee的行为，对象Adapter是通过代理转发请求到Adaptee来获取Adaptee的行为。Adapter模式关键在于:
- 通过继承或者持有Adaptee的引用来获取行为，而Adapter通过继承Target因此拥有Target的接口
- 由于Java语言不能够多重继承，因此对于类适配器模式，Target需要是一个接口

#### 示例代码
通过提供Shirt原型 Person展示不同的穿着
```
public interface Shirt {
  void show();
}

public class RedShirt implements Shirt{
  @Override
  public void show() {
    System.out.println("This is red shirt.");
  }
}

public interface ShirtAdaptee {
  void display();
}

public class RedShirtAdaptee implements ShirtAdaptee {
  @Override
  public void display() {
    System.out.println("This is red shirt.");
  }
}

public class ShirtAdaptor implements Shirt {
  private ShirtAdaptee shirtAdaptee;

  public ShirtAdaptor(ShirtAdaptee shirtAdaptee) {
    this.shirtAdaptee = shirtAdaptee;
  }

  @Override
  public void show() {
    this.shirtAdaptee.display();
  }
}

public class ClassRedShirtAdaptor extends RedShirtAdaptee implements Shirt {
  @Override
  public void show() {
    this.display();
  }
}

public class Person {

  void print(Shirt shirt) {
    shirt.show();
  }

  public static void main(String[] args) {
    Person person = new Person();

    Shirt shirt = new RedShirt();

    ShirtAdaptee shirtAdaptee = new RedShirtAdaptee();
    Shirt shirtAdaptor = new ShirtAdaptor(shirtAdaptee);

    ClassRedShirtAdaptor classRedShirtAdaptor = new ClassRedShirtAdaptor();
    person.print(shirt);
    person.print(shirtAdaptor);
    person.print(classRedShirtAdaptor);
  }
}
```
