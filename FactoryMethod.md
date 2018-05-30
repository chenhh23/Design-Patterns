
### Factory Method(工厂方法)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/FactoryMethod.png)

#### 意图

定义一个创建对象的方法， 具体创建什么样的对象延迟到子类来决定。

#### 使用场景

- 父类事先不能准确知道需要创建什么样的对象，只知道需要创建对象的接口，并对该对象接口进行操作， 再由子类来提供创建具体对象的方法

#### 优缺点

- 代码中不需要依赖具体的类，而只需要依赖接口，操作接口中的方法即可，具体类由子类去指定和创建

- 一些稍稍有差别的具体类都需要重新创建一个子类去对应创建，容易造成子类大量的创建。可以提供一个带有参数的make方法，方法里面根据参数来创建Product来实现减少子类的作用

#### 想法

面对接口编程的一种实现，这个是5种创建型设计模式中唯一一种以类区分的设计模式，它的实现是通过class的继承，因此带来的一个弊端就是涉及到改变的时候都需要通过继承来拓展，因而可能会造成很多子类。关键点在于:

- 父类操作依赖于Product的接口，在子类中实现具体Product的创建

#### 示例代码

```

public interface Shirt {
  void print();
}

public class GreenShirt implements Shirt {
  @Override
  public void print() {
    System.out.println("This is green shirt.");
  }
}

public class RedShirt implements Shirt {
  @Override
  public void print() {
    System.out.println("This is red shirt.");
  }
}

public abstract class Person {
  void show() {
    Shirt shirt = getShirt();
    shirt.print();
  }

  abstract Shirt getShirt();
}

public class GreenShirtPerson extends Person {
  @Override
  Shirt getShirt() {
    return new GreenShirt();
  }
}

public class RedShirtPerson extends Person {
  @Override
  Shirt getShirt() {
    return new RedShirt();
  }
}

public class ClothesShow {
  public static void main(String args[]) {
    Person redPerson = new RedShirtPerson();
    Person greenPerson = new GreenShirtPerson();
    redPerson.show();
    greenPerson.show();
  }
}
```
