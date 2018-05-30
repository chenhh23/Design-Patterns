
### Singleton(单例模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/singleton.png)

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
