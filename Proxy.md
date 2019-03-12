### Proxy(代理模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/proxy.png)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/proxy.png)

#### 意图

通过代理对象并转发请求从而达到控制对这个对象的访问

#### 使用场景
- 虚代理，对一个对象进行控制访问，只有当我们确实需要用到这个对象时，才对它进行创建和初始化
- 保护代理，控制对原始对象的访问，将权限控制做在代理对象中
- 智能指引，访问对象时需要做一些额外的辅助操作，如计数等

#### 优缺点
- 通过代理引入间接性，在间接性当中做一些逻辑处理
- 虚代理可以对程序进行优化，只有当确实使用到这个对象时才进行创建和初始化，从而减少启动时响应时间
- 保护代理和智能指引能够在访问对象时插入一些额外的辅助操作

#### 想法
Proxy模式主要关键点是通过代理向原对象转发请求，从而引入间接性，在这个间接性当中可以做一些逻辑处理，如权限控制，计数等。另一方面是通过间接性来延迟原对象的创建，通过持有对象的间接引用，当确实需要用到该对象时才进行创建，从而减少了启动时的创建开销。

#### 示例代码
延迟商店里面衣服的创建，当需要展示时才创建相应的对象
```
public interface Shirt {
  void show();
}

public class NikeShirt  implements Shirt {
  @Override
  public void show() {
    System.out.println("this is nike shirt");
  }
}

public class AdidasShirt implements Shirt {
  @Override
  public void show() {
    System.out.println("this is adidas shirt");
  }
}


public class ShirtProxy implements Shirt {
  private Shirt shirt;
  private String brand;

  public ShirtProxy(String brand) {
    this.brand = brand;
  }

  @Override
  public void show() {
    if (shirt == null) {
      if (brand.equals("adidas")) {
        shirt = new AdidasShirt();
      } else if (brand.equals("nike")) {
        shirt = new NikeShirt();
      } else {
        throw new RuntimeException("brand not match");
      }
    }
    shirt.show();
  }
}

public class ShirtStore {
  private List<Shirt> shirts;

  public ShirtStore() {
    this.shirts = new ArrayList<>();
  }

  void add(Shirt shirt) {
    this.shirts.add(shirt);
  }

  void print() {
    this.shirts.stream().forEach(shirt -> shirt.show());
  }

  public static void main(String args[]) {
    ShirtStore shirtStore = new ShirtStore();
    shirtStore.add(new ShirtProxy("nike"));
    shirtStore.add(new ShirtProxy("adidas"));
    shirtStore.print();
  }
}


```
