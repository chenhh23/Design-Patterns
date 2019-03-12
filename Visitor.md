### Visitor(访问者模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/visitor.png)

#### 意图

访问者模式提供作用于本对象某元素的操作，本对象通过提供接受特定访问者的入口，从而将本对象某元素暴露给访问者操作。可以在不改变元素类的前提下，重新定义访问者从而重定义操作。

#### Key
- Visitor 为对象结构的每个类提供访问的操作，通过具体类结构名字和参数决定特定的被访问的对象
- Element 定义一个accept操作，接受接受访问者
- ConcreteElement 实现accept的操作，接受具体的访问者，执行特定的访问操作函数

#### 使用场景
- 一个对象结构包含很多不同对象，分别有不同的接口，想对这些对象进行一些依赖于其他类的操作
- 对象结构比较稳定，很少添加一些拓展的子类，而且避免visitor接口都进行添加新的操作
- 需要对对象结构中的对象进行很多不同的操作，又想避免直接访问对象当中的数据，可以提供一个访问者的接口，访问对象中的数据。

#### 优缺点
- 访问者模式可以简单的增加新的操作，只需要重定义一个新的visitor类即可实现新的操作
- 添加新的ConcreteElement会导致 所有的visitor接口都需要添加操作，以应对这个新的ConcreteElement
- 将操作集中在访问者上面，可以进行统一的修改和管理

#### 想法
Visitor模式主要是为对象结构提供一个能够操作对象的Visitor，对象结构通过accept 接受访问者，从而将自身作为参数传递给访问者操作。每一个具体的对象都需要有一个具体的访问者操作处理它，这使得添加新的Element子类时，所有的Visitor类与其子类均需要修改。

#### 示例代码
不同的顾客访问不同商店
```
public interface Store {
  void accept(Customer customer);
}

public class NikeStore implements Store {
  @Override
  public void accept(Customer customer) {
    customer.visitNike(this);
  }

  public String name() {
    return "Nike Store";
  }
}

public class AdidasStore implements Store {
  @Override
  public void accept(Customer customer) {
    customer.visitAdidas(this);
  }

  public String name() {
    return "Adidas Store";
  }
}

public interface Customer {
  void visitAdidas(AdidasStore adidasStore);

  void visitNike(NikeStore nikeStore);
}

public class ACustomer implements Customer {
  @Override
  public void visitAdidas(AdidasStore adidasStore) {
    System.out.println("I am A customer");
    System.out.println("I reach " + adidasStore.name());
    System.out.println("I want to buy something.");
  }

  @Override
  public void visitNike(NikeStore nikeStore) {
    System.out.println("I am A customer");
    System.out.println("I reach " + nikeStore.name());
    System.out.println("I want to buy something.");
  }
}

public class BCusomter implements Customer {
  @Override
  public void visitAdidas(AdidasStore adidasStore) {
    System.out.println("I am B customer");
    System.out.println("I reach " + adidasStore.name());
    System.out.println("I want to rob.");
  }

  @Override
  public void visitNike(NikeStore nikeStore) {
    System.out.println("I am B customer");
    System.out.println("I reach " + nikeStore.name());
    System.out.println("I want to rob.");
  }
}

public class Test {
  public static void main(String args[]) {
    Customer a = new ACustomer();
    Customer b = new BCusomter();
    NikeStore nike = new NikeStore();
    AdidasStore adidas = new AdidasStore();
    a.visitAdidas(adidas);
    a.visitNike(nike);

    b.visitAdidas(adidas);
    b.visitNike(nike);
  }
}

```
