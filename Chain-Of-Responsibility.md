### Chain Of Responsibility(责任链模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/chain-of-responsibility1.png)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/chain-of-responsibility2.png)

#### 意图

避免请求的发送者和接收者耦合，使得多个对象能够有机会处理请求。将接收者连接成对象的链，请求在链上流动直至有一个对象处理它。

#### 使用场景
- 多个对象可以处理同一个请求，不知道哪个对象真正对这个请求感兴趣
- 在不明确指定接收者的情况下，向多个接收者提交一个请求
- 可以处理对象的集合应该被动态的指定

#### 优缺点
- 降低发送者和接收者的耦合性，发送者不需要明确指定某个接收者处理请求
- 增加了接收者的灵活性，能够动态的给请求指定接收者
- 不保证请求一定被接受，有可能请求到了链的末端也没被处理

#### 想法
Chain Of Responsibility模式主要是实现请求者和接收者的解耦，同一个请求可以动态的选择潜在多个接受者进行处理，当请求在责任链中流动并被某个接收者处理时就返回，但不能够保证请求一定被处理。责任链可以采用已存在的链，也可以新建符合需求的链，其中一种拓展是，在Client里面把所有的链都建好。

#### 示例代码
处理请求链
```
public abstract class Handler {
  private Handler successor;

  public Handler getSuccessor() {
    return successor;
  }

  public void setSuccessor(Handler successor) {
    this.successor = successor;
  }

  public abstract void handleRequest();
}

public class ConcreteHandler extends Handler {

  @Override
  public void handleRequest() {
    if (getSuccessor() != null) {
      System.out.println("pass.");
      getSuccessor().handleRequest();
    } else {
      System.out.println("process.");
    }
  }
}

public class Test {
  public static void main(String args[]) {
    Handler aHandler = new ConcreteHandler();
    Handler bHandler = new ConcreteHandler();
    Handler cHandler = new ConcreteHandler();

    aHandler.setSuccessor(bHandler);
    bHandler.setSuccessor(cHandler);
    aHandler.handleRequest();
  }
}

  ```
