### State(状态模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/state.png)

#### 意图

状态模式通过持有状态引用来影响它的行为，在其内部状态改变时改变它的行为，对象看起来像是修改了它的类。

#### Key
- Context 定义对外的客户接口，同时维护一个具体的状态接口实例
- State 定义接口，封装与Context的特定状态相关的行为
- ConcreteState 子类实现与Context的特定状态相关的行为

#### 使用场景
- 对象的行为取决于状态，行为随着状态的改变而改变
- 对于多分支情况，并且每个分支的判断条件是一个状态的枚举，可以通过每一个条件分支放入一个独立的类中，从而消除了庞大复杂的多分支结构

#### 优缺点
- 将不同状态的行为分割开来 通过将所有与特定状态相关的行为放入一个对象当中。通过定义新的子类，从而可以很容易的增加新的状态和转换。
- 状态转换显式化 通过对状态对象引用的转换，从而显式，一致性的修改Context的状态
- State对象可被共享 如果State对象没有实例变量，那么多个Context可以共用同一个State对象，并且能够保持状态的一致性

#### 想法
State模式主要是Context对于不同State子类的引用，从而通过改变内部状态来改变对外表现的行为。对于同一个对象，状态不同导致行为不同，从而像是改变了对象的类。

#### 示例代码
商店开关门导致的状态变化
```
public interface State {
  void open(Store store);

  void close(Store store);

  void changeState(Store store, State state);
}

public class Store {
  private State state;
  private String name;

  public Store(State state, String name) {
    this.state = state;
    this.name = name;
  }

  public void close() {
    state.close(this);
  }

  public void open() {
    state.open(this);
  }

  public void changeState(State state) {
    this.state = state;
  }

  public String getName() {
    return name;
  }
}

public class OpenState implements State {
  @Override
  public void open(Store store) {
    System.out.println(store.getName() + " has been opened.");
  }

  @Override
  public void close(Store store) {
    System.out.println(store.getName() + " will be closed.");
    changeState(store, new CloseState());
  }

  @Override
  public void changeState(Store store, State state) {
    store.changeState(state);
  }
}

public class CloseState implements State {
  @Override
  public void open(Store store) {
    System.out.println(store.getName() + " will be opened.");
    changeState(store, new OpenState());
  }

  @Override
  public void close(Store store) {
    System.out.println(store.getName() + " has been closed.");
  }

  @Override
  public void changeState(Store store, State state) {
    store.changeState(state);
  }
}

public class Test {
  public static void main(String args[]) {
    State open = new OpenState();
    Store store = new Store(open, "Store A");
    store.open();
    store.close();
    store.close();
    store.open();
  }
}

```

