### Mediator(中介者模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/mediator.png)

#### 意图

中介者模式通过一个中介的对象来封装一系列对象之间的交互，使得一系列对象之间的交互不需要显式的相互引用，从而使得其耦合松散，能够独立地改变一系列对象之间的交互。

#### Key
- Mediator 中介者接口定义了接口用于与各个对象之间的交互
- ConcreteMediator 具体的中介者协调各对象实现行为
- Colleague 同事类知道中介者对象，需要与其他同事类通讯时，与中介者通讯

#### 使用场景
- 一系列对象之间进行复杂的通讯，产生的相互依赖关系结构混乱复杂
- 一个对象引用很多对象，导致该对象难以复用

#### 优缺点
- 减少了子类的生成 Mediator将原本分布于多个对象间的行为集中在一起，只需要修改Mediator类即可
- 各个同事类之间解耦，他们只需要知道Mediator类即可
- 将多对多之间的交互简化成一对多
- 控制集合在中介者上，将交互的复杂性转化成中介者的复杂性

#### 想法
Mediator模式主要是通过中介者一对多交互来简化原本一系列对象多对多交互的复杂性，将对象之间交互的复杂性转化成中介者的复杂性，从而解决了Colleague对象之间复杂的引用导致对象的难以复用。

#### 示例代码
在buyer和seller中充当中介者
```
public interface AbstractMediator {
  void listen(Person person);

  void contactPerson();
}

public class Mediator implements AbstractMediator {
  private Buyer buyer;
  private Seller seller;

  @Override
  public void listen(Person person) {
    if (person == buyer) {
      System.out.println(buyer.getMessage());
      System.out.println(seller.answer());
    } else if (person == seller) {
      System.out.println(seller.getMessage());
      System.out.println(buyer.answer());
    }
  }

  @Override
  public void contactPerson() {
    buyer = new Buyer(this);
    seller = new Seller(this);
  }

  public Buyer getBuyer() {
    return buyer;
  }

  public Seller getSeller() {
    return seller;
  }
}

public abstract class Person {
  protected AbstractMediator mediator;

  public Person(AbstractMediator mediator) {
    this.mediator = mediator;
  }

  public abstract String answer();

  public abstract void say(String message);
}

public class Buyer extends Person {
  private String message;

  public Buyer(AbstractMediator mediator) {
    super(mediator);
  }

  public String getMessage() {
    return message;
  }

  @Override
  public String answer() {
    return "Yes, I want.";
  }

  @Override
  public void say(String message) {
    this.message = message;
    mediator.listen(this);
  }
}

public class Seller extends Person {
  private String message;

  public Seller(AbstractMediator mediator) {
    super(mediator);
  }

  public String getMessage() {
    return message;
  }

  public String answer() {
    return "Don't call to me, I will contact you if I want.";
  }

  @Override
  public void say(String message) {
    this.message = message;
    mediator.listen(this);
  }
}

public class Test {
  public static void main(String args[]) {
    Mediator mediator = new Mediator();
    mediator.contactPerson();
    Person buyer = mediator.getBuyer();
    Person seller = mediator.getSeller();
    buyer.say("I want to buy.");
    seller.say("Do you want to buy?");
  }
}


```


