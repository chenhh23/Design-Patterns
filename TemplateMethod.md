### Template Method(模板方法模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/template-method.png)

#### 意图

模板方法模式是定义一个操作中算法的骨架，将一些步骤延迟到子类当中，提供算法步骤的拓展点，可以不改变算法的结构即可重定义算法的某些定义

#### Key
- AbstractClass 定义一个算法的骨架，并保留算法的拓展点，通过子类去实现这些步骤
- ConcreteClass 子类实现特定算法步骤

#### 使用场景
- 定义一个算法骨架，实现算法中不变的部分，把需要改变的部分让子类去实现
- 子类的公共行为提取出来，分离需要改变和不需要改变的部分，让需要改变的部分给子类去实现

#### 优缺点
- 能够控制算法的拓展点，子类实现的操作在控制的特定点起作用
- 代码复用，父类实现公共部分，将需要修改部分操作留给子类实现
- 命名有一般约定，需要被重定义的操作一般用Do前缀来识别。

#### 想法
Template Method模式主要是定义一个算法的骨架，并留有一些需要重定义的操作让子类具体去实现，将步骤的实现延迟到子类当中。

#### 示例代码
用不同交通工具去旅游
```
public abstract class Travel {
  public void travel() {
    System.out.println("I am going to Travel.");
    takeVehicle();
    System.out.println("I has been arrived.");
  }

  public abstract void takeVehicle();
}

public class TrainTravel extends Travel {
  @Override
  public void takeVehicle() {
    System.out.println("Take train.");
  }
}

public class PlaneTravel extends Travel {
  @Override
  public void takeVehicle() {
    System.out.println("Take a plane");
  }
}

public class Test {
  public static void main(String args[]) {
    Travel planeTravel = new PlaneTravel();
    Travel trainTravel = new TrainTravel();
    planeTravel.travel();
    trainTravel.travel();
  }
}

```


