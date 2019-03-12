### Observer(观察者模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/observer.png)

#### 意图

观察者模式定义了对象间一种一对多的通知机制，当一个对象状态发生了改变，所有它的观察者都会得到通知并进行更新。

#### Key
- Subject 目标知道它的观察者。提供注册和删除观察者对象的接口
- Observer 观察者，目标改变时需要通知的对象

#### 使用场景
- 存在一对多的通知关系，当一个对象修改时，需要通知其他对象进行相应的修改，而又不知道具体有多少个对象有待修改
- 两个对象不是紧密耦合的，一个对象修改时需要其他对象作相应的修改，但是该对象又不是确定的，是可以动态加入的

#### 优缺点
- 目标和观察者之间是抽象耦合的。 目标只需要直接它的观察者符合Observer的接口，而并不需要知道具体是什么类。
- 目标和观察者之间是相互独立的。 可以分别各自复用目标和观察者，而不需要同时影响两者
- 支持广播。 观察者可以动态的加入或者退出对某个目标的观察

#### 想法
Observer模式主要是包含Observer和Subject两种角色，Subject作为事件的发布者，Observer作为事件的监听者，当目标对象的状态发生改变时，通知Observer进行更新。这是一种一对多的通知关系，Subject只需要向它的观察者发布通知，Observer可以动态的加入或者退出某个目标的观察。

#### 示例代码
```
public interface Person {
  void read(String message);
}

public class APerson implements Person {
  private NewsPaper paper;

  public APerson(NewsPaper paper) {
    paper.register(this);
    this.paper = paper;
  }

  @Override
  public void read(String message) {
    System.out.println("A Person accept message:" + message);
  }
}

public class BPerson implements Person {
  private NewsPaper paper;

  public BPerson(NewsPaper paper) {
    paper.register(this);
    this.paper = paper;
  }

  @Override
  public void read(String message) {
    System.out.println("B Person accept message:" + message);
  }
}

public class NewsPaper {
  private List<Person> persons;

  public NewsPaper() {
    this.persons = new ArrayList<>();
  }

  public void register(Person person) {
    this.persons.add(person);
  }

  public void remove(Person person) {
    this.persons.remove(person);
  }

  public void notify(String message) {
    for (Person person : persons) {
      person.read(message);
    }
  }
}

public class Test {
  public static void main(String args[]) {
    NewsPaper paper = new NewsPaper();
    Person a = new APerson(paper);
    Person b = new BPerson(paper);
    paper.notify("It is 1 am");
    paper.remove(a);
    paper.notify("It is 2 am");
  }
}

```

