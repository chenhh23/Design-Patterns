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

### Builder(生成器模式)

![image](http://chimegitlab.d.chime.me/1go/design-patterns/uploads/4bd1a4c8b9faf0b6dd0e3153bc71ccd5/image.png)

#### 意图

一般用于复杂对象的构建, 将复杂对象的构建过程与它的最终形态分离开来

#### 使用场景

- 复杂对象构建过程中可以允许有不同阶段的展示形态

- 复杂对象的表示与其构建，装配过程能够分离开来, Builder关注于对象的构建和装配, 而Director提供创建的算法以及创建的参数。

- 同一套创建的算法以及创建参数可以应用于不同的Builder创建不同的复杂对象

#### 优缺点

- 对象的表示和其构造分离, 意味着对于同一个对象的表示和它的创建算法可以复用多个Builder实现不同的构建和装配

- 对于构造对象的过程能够更加精细的进行控制，但也需要在获取构造好的产品时进行控制，避免返回一些不允许的残次品

- 因为Builder装配的产品差别比较大，没有一个公共的抽象Product类, 所以对象的表示需要依赖到Builder返回的具体的Product

#### 想法

Builder模式的作用不仅仅局限于我们平时所用到的用Builder去构造一个Model, 这仅仅是使用到了它控制精细构建过程的一小部分作用。关键点还在于:

- 复杂对象的创建算法与它的构建、装配过程分离，从而达到创建算法复用多个Builder，构建不同的复杂对象

#### 示例代码

根据用户Name和邮箱 构造欢迎邮件和回复邮件。

```
// 邮件构造器接口
public interface EmailBuilder {
  void buildName(String name);

  void buildEmail(String email);
}

// 欢迎邮件
public class WelcomeEmailBuilder implements EmailBuilder {
  private String body = "Hi, %s Welcome to join us. your email address: %s";

  private String name;
  private String email;

  @Override
  public void buildName(String name) {
    this.name = name;
  }

  @Override
  public void buildEmail(String email) {
    this.email = email;
  }

  public String build() {
    Objects.requireNonNull(name);
    Objects.requireNonNull(email);
    return String.format(body, name, email);
  }
}

// 回复邮件
public class ReplyEmailBuilder implements EmailBuilder {
  private String body = "%s, Thanks to your reply!";

  private String name;

  @Override
  public void buildName(String name) {
    this.name = name;
  }

  @Override
  public void buildEmail(String email) {

  }

  public String build() {
    Objects.requireNonNull(name);
    return String.format(body, name);
  }
}

// builder构造邮件
public class Person {
  private String name;
  private String email;

  public Person(String name, String email) {
    this.name = name;
    this.email = email;
  }

  void buildEmail(EmailBuilder builder) {
    builder.buildEmail(email);
    builder.buildName(name);
  }

  public static void main(String args[]) {
    Person person = new Person("huahuan", "huahuan.chen@renren-inc.com");
    WelcomeEmailBuilder welcomeEmailBuilder = new WelcomeEmailBuilder();
    ReplyEmailBuilder replyEmailBuilder = new ReplyEmailBuilder();
    person.buildEmail(welcomeEmailBuilder);
    person.buildEmail(replyEmailBuilder);
    System.out.println(welcomeEmailBuilder.build());
    System.out.println(replyEmailBuilder.build());
  }
}
```

### Factory Method(工厂方法)

![image](http://chimegitlab.d.chime.me/1go/design-patterns/uploads/74bde6aa1b74a18c4c6670d20d9f3b8d/image.png)

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

### Prototype(原型)

![image](http://chimegitlab.d.chime.me/1go/design-patterns/uploads/66cda16bf3da6a87392ce3ed0b32e1de/image.png)

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

### Singleton(单例模式)

![image](http://chimegitlab.d.chime.me/1go/design-patterns/uploads/fbbb795e2c1905b2f6ca5bf46d1e1bb8/image.png)

#### 意图

提供全局能够访问类的一个实例

#### 使用场景

- 类只需要有一个实例，并且能够全局访问得到

#### 优缺点

- 可以控制单例的访问入口

- 多线程时需要考虑单例实现的性能和线程安全

#### 想法

单例模式主要提供的单实例的实现，实现类的实例的控制访问。针对于Java的多线程来说，还需要提供锁来实现安全的单例模式。除此之外，还可以提供注册表的形式提供单例的管理

#### 示例代码

```

public class GreenShirt implements Shirt {
  private static volatile GreenShirt shirt;

  private GreenShirt() {

  }

  @Override
  public void print() {
    System.out.println("This is green shirt.");
  }

  public static GreenShirt instance() {
    if (shirt != null) {
      synchronized (GreenShirt.class) {
        if (shirt != null) {
          shirt = new GreenShirt();
        }
      }
    }
    return shirt;
  }
}

public class RedShirt implements Shirt {
  private static volatile RedShirt shirt;

  private RedShirt() {

  }

  @Override
  public void print() {
    System.out.println("This is red shirt.");
  }

  public static RedShirt instance() {
    if (shirt != null) {
      synchronized (RedShirt.class) {
        if (shirt != null) {
          shirt = new RedShirt();
        }
      }
    }
    return shirt;
  }
}
```
