### Facade(外观模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/facade.png)

#### 意图

提供一个高层的接口，这个高层的接口处理子系统的关系，以此使得子系统更加易于使用。

#### 使用场景
- 为复杂的子系统提供定制化简单的接口，如果需要更自由的定制化可以直接越过facade层
- facade作为一个层次结构子系统的入口点，简化子系统之间的相互依赖关系

#### 优缺点
- Facade将请求转发给子系统处理，子系统没有facade的相关信息
- Facade屏蔽了子系统组件，使得客户程序使用起来更加方便
- 实现了客户程序和子系统的松耦合，消除了复杂的循环依赖关系，能够建立起层次结构系统

#### 想法
Facade模式主要点是能够降低客户程序和子系统之间的耦合度，通过屏蔽子系统组件，而使得facade跟客户程序之间的通讯，再将请求转发给子系统处理，客户程序不需要知道是哪个子系统处理的请求。这样使得子系统使用起来更加方便，也能够消除子系统之间复杂的循环依赖关系，从而作为层次结构的入口点，建立起子系统的层次结构系统。

#### 示例代码
项目中的service 使用到各部分数据表相当于一个facade

```
public class SubSystemA {
  public void actionA() {
    System.out.println("exec action A");
  }
}

public class SubSystemB {
  public void actionB() {
    System.out.println("exec action B");
  }
}

public class SubSystemC {
  public void actionC() {
    System.out.println("exec action C");
  }
}

public class Facade {
  private SubSystemA a;
  private SubSystemB b;
  private SubSystemC c;

  public Facade(SubSystemA a, SubSystemB b, SubSystemC c) {
    this.a = a;
    this.b = b;
    this.c = c;
  }

  public void method() {
    a.actionA();
    b.actionB();
    c.actionC();
  }

}

public class Test {
  public static void main(String args[]) {
    SubSystemA a = new SubSystemA();
    SubSystemB b = new SubSystemB();
    SubSystemC c = new SubSystemC();
    Facade facade = new Facade(a, b, c);
    facade.method();
  }
}


```
