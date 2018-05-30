# Design-Patterns
### Abstract Factory(抽象工厂模式)

![image](http://chimegitlab.d.chime.me/1go/design-patterns/uploads/3d67f53dc8c0a0f88eb411cee3d44b94/image.png)

#### 意图

提供一个创建`一系列`相关对象的接口

#### 使用场景

- 约束的是一系列对象，需要更改时候，这一系列对象同时更改

- Client调用只需要关注Product和Factory的接口，而不需要知道具体有哪些产品和工厂会被构造，产品的构造过程等

#### 优缺点

- 能够保持一系列产品的配套，更换的时候能够同时一起更换

- 面向接口编程的实现，Client调用只需要知道接口，而不依赖于创建的具体细节

- 面对可能以后需要增加的Product种类处理不够灵活, 相当于AbstractFactory需要增加一个新的构造Product的方法, 这就不得不通过AbstractFactory以及它的所有子类

#### 想法

这是面对接口编程的一种实现, 关键点主要有两方面:

1. 保持一系列产品的配套创建和使用，能够控制和管理Product的组合，而避免出现不搭配的现象

1. Client调用只需要依赖Factory和Product接口, 而不关心Factory和Product的创建以及实现

1. 虽然AbstractFactory可以是一个提供默认实现方法的Class，但是针对于Java语言来说, AbstractFactory是一个Interface更加通用灵活, 因为Java里面不能够多继承

1. 体现了依赖倒置的设计原则，违反了迪米特原则

#### 示例代码

一个人白天穿一整套红色衣服，晚上穿一整套绿色衣服。使用抽象工厂控制配套，不至于会造成红配绿的情况

```
// 裤子interface
public interface Pants {
  void print();
}
// 红裤子
public class RedPants implements Pants {
  @Override
  public void print() {
    System.out.println("This is red pant.");
  }
}
// 绿裤子
public class GreenPants implements Pants {
  @Override
  public void print() {
    System.out.println("This is green pant.");
  }
}

// 衬衫
public interface Shirt {
  void print();
}

// 红衬衫
public class RedShirt implements Shirt {
  @Override
  public void print() {
    System.out.println("This is red shirt.");
  }
}

// 绿衬衫
public class GreenShirt implements Shirt {
  @Override
  public void print() {
    System.out.println("This is green shirt.");
  }
}

// 衣服抽象工厂
public interface ClothesFactory {
  Shirt getShirt();

  Pants getPants();
}

// 红色整套
public class RedClothesFactory implements ClothesFactory {
  @Override
  public Shirt getShirt() {
    return new RedShirt();
  }

  @Override
  public Pants getPants() {
    return new RedPants();
  }
}

// 绿色整套
public class GreenClothesFactory implements ClothesFactory {
  @Override
  public Shirt getShirt() {
    return new GreenShirt();
  }

  @Override
  public Pants getPants() {
    return new GreenPants();
  }
}

// 人穿衣服
public class Person {
  void wear(ClothesFactory clothesFactory) {
    Shirt shirt = clothesFactory.getShirt();
    Pants pants = clothesFactory.getPants();
    shirt.print();
    pants.print();
  }

  public static void main(String args[]) {
    Person person = new Person();
    // morning
    person.wear(new RedClothesFactory());
    // evening
    person.wear(new GreenClothesFactory());
  }

}
```
