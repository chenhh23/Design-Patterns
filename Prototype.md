
### Prototype(原型)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/prototype.png)

#### 意图

可以提供一个原型模板通过copy来创建对象实例，通过提供不同的原型模板创建不同对象实例

#### 使用场景

- 通过copy来创建对象实例，从而减少工厂方法模式中因为具体创建对象而创建不同的工厂类

- 对象层次的设计模式，因此可以在运行时通过传入不同的原型模板来创建对应的对象实例

#### 优缺点

- 能够在运行时指定需要创建的对象实例类型

- 通过提供不同的原型模板实现创建不同的对象实例，从而减少因为需要创建不同种类的Product而导致不同工厂子类的创建

- 需要prototype原型均实现clone的操作，而对于已存在的类，如果其未实现clone操作，则需要较大的改造，对于复杂对象，包含其他对象的实例，则需要考虑深拷贝的问题

#### 想法

prototype模式可以应用的地方，使用Abstract Factory、Factory Method、Builder 都有它的可行之处，只不过对应的另外三种模式会导致大量的子类生成。prototype模式关键在于:

- 提供原型模板通过copy来创建对象实例，提供不同的原型模板创建不同的对象实例从而不需要创建相应的工厂子类，从而减少类的数量

- prototype模式需要依赖clone方法的实现

#### 示例代码

通过提供Shirt原型 Person展示不同的穿着

```
public interface Shirt extends Cloneable {
  Shirt clone();

  void print();
}

public class RedShirt implements Shirt {
  @Override
  public void print() {
    System.out.println("This is red shirt.");
  }

  @Override
  public Shirt clone() {
    return new RedShirt();
  }
}

public class GreenShirt implements Shirt {
  @Override
  public void print() {
    System.out.println("This is green shirt.");
  }

  @Override
  public Shirt clone() {
    return new GreenShirt();
  }
}

public class Person {
  private Shirt shirt;

  public Person(Shirt shirt) {
    this.shirt = shirt;
  }

  void show() {
    Shirt cloneShirt = shirt.clone();
    cloneShirt.print();
  }
}
```
