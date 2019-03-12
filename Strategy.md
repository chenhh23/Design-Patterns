### Strategy(策略模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/strategy.png)

#### 意图

策略模式定义一系列算法，把他们一个个封装起来，使得它们可以动态替换。算法可以独立客户而发送改变。

#### Key
- Strategy 定义所有支持算法的公共接口
- Context 维护一个对Strategy对象的引用
- ConcreteStrategy 具体的实现了算法的类

#### 使用场景
- 相关的类仅仅是行为不同, 行为可以动态修改
- 需要使用一个算法不同的变体 
- 一个类使用多个条件语句定义了多种行为，可以通过将相关的条件分支放入各种的strategy类中，从而消除条件语句

#### 优缺点
- 算法可复用，可动态替换
- 使用组合代替继承，即实现了代码复用，又能够实现动态的替换，不需要硬编码，而难以拓展
- context可以根据需求，选择不同的算法实现
- 能够消除一些复杂的条件语句，从而编写出可拓展可维护可测试的代码
- 添加了对象的数目 

#### 想法
Strategy模式主要是Context对于不同Strategy子类的引用，通过改变算法，来改变行为。这一系列算法可动态的被替换和拓展，从而实现了算法的复用。对于context需要共享给strategy的消息，可以通过接口参数传递。

#### 示例代码
dog 叫声随着算法而改变
```
public interface Animal {
  void bark();
}

public class Dog implements Animal {
  private Strategy strategy;

  public Dog(Strategy strategy) {
    this.strategy = strategy;
  }

  @Override
  public void bark() {
    this.strategy.bark();
  }

  public Strategy getStrategy() {
    return strategy;
  }

  public void setStrategy(Strategy strategy) {
    this.strategy = strategy;
  }
}

public interface Strategy {
  void bark();
}

public class MiaoMiaoStrategy implements Strategy {
  @Override
  public void bark() {
    System.out.println("miao miao!");
  }
}

public class WangWangStrategy implements Strategy {
  @Override
  public void bark() {
    System.out.println("wang wang!");
  }
}

public class Test {
  public static void main(String args[]) {
    Strategy miaoStrategy = new MiaoMiaoStrategy();
    Strategy wangStrategy = new WangWangStrategy();
    Dog dog = new Dog(miaoStrategy);
    dog.bark();
    dog.setStrategy(wangStrategy);
    dog.bark();
  }
}

```
