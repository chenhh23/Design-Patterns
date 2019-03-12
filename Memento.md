### Memento(备忘录模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/memento.png)

#### 意图

备忘录模式在不破坏封装性的前提下，捕获对象的内部状态，并在该对象之外保存这个状态，这样以后可将该对象恢复至原来保存的状态。

#### Key
- Memento 备忘录存储原发器对象的内部状态
- Originator 原发器创建一个备忘录，用来记录当前时刻的内部状态以及使用备忘录恢复原来的内部状态
- Caretaker 负责保存备忘录，不能对备忘录进行操作或者检查

#### 使用场景
- 需要保存一个对象的内部状态，以后需要时能够恢复原来的状态
- 不能破坏对象的实现细节并破坏对象的封装性

#### 优缺点
- 保护对象的封装性，备忘录模式可以把原发器内部信息对其他对象屏蔽起来
- 备忘录代价可能很高，如果对象内部状态复杂，频繁的创建和恢复原发器状态可能会导致非常大的开销
- 维护备忘录有潜在的代价 管理器负责删除所维护的备忘录，但是管理器并不知道备忘录中有多少个状态，可能会产生大量的存储开销

#### 想法
Memento模式主要是通过原生器生成备忘录，备忘录只能由原生器访问、操作和检查，由管理器负责维护备忘录的存储。

#### 示例代码
```
public class Originator {
  private String state;

  public Originator(String state) {
    this.state = state;
  }

  public Memento createMemento() {
    return new Memento(state);
  }

  public void setMemento(Memento memento) {
    state = memento.getState();
  }

  public String getState() {
    return state;
  }

  public void setState(String state) {
    this.state = state;
  }
}

public class Memento {
  private String state;

  public Memento(String state) {
    this.state = state;
  }

  public String getState() {
    return state;
  }
}

public class Caretaker {
  private Memento memento;

  public Caretaker(Memento memento) {
    this.memento = memento;
  }

  public Memento getMemento() {
    return memento;
  }

  public void setMemento(Memento memento) {
    this.memento = memento;
  }
}

public class Test {
  public static void main(String args[]) {
    Originator originator = new Originator("State A");
    System.out.println(originator.getState());
    Memento memento = originator.createMemento();
    Caretaker caretaker = new Caretaker(memento);
    originator.setState("State B");
    System.out.println(originator.getState());
    originator.setMemento(caretaker.getMemento());
    System.out.println(originator.getState());
  }
}

```

