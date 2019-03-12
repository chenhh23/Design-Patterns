### Composite(组合模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/composite1.png)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/composite2.png)

#### 意图

利用树形数据结构处理整体和局部关系，使得用户能够一致性处理单个对象和组合对象

#### 使用场景
- 利用树形数据结构处理整体和局部的关系
- 希望单一对象和组合对象能够有一致性的处理方式

#### 优缺点
- 包含了单一对象和组合对象的结构，用户能够一致性的对待这些对象，而不用关心他们的区别，简化了用户的代码
- 添加新的组件类型简单，不会因为新的component子类影响原本的树形结构
- 限制组合对象中的组件类型需要额外的检验
- interface需要定义额外的操作，这些操作leaf component不会用到，需要针对这些函数进行处理

#### 想法
Composite模式主要点是能够利用树形数据结构来处理 整体和局部的关系，使得用户能够一致化的处理组合对象和单一对象。一般来说，组件对于用户来说是透明的，不需要关注他们是组合还是单一对象，但是需要最大化component的接口，以此来一致化处理这两种组件。

#### 示例代码
商城、商店、衣服 三种不同层次类一致化展示
```
public interface Shirt {
  void show();
  void add(Shirt shirt);
  void remove(Shirt shirt);
}

public class AdidasShirt implements Shirt {
  @Override
  public void show() {
    System.out.println("This is adidas shirt.");
  }

  @Override
  public void add(Shirt shirt) {
    throw new UnsupportedOperationException();
  }

  @Override
  public void remove(Shirt shirt) {
    throw new UnsupportedOperationException();
  }
}

public class NikeShirt implements Shirt{
  @Override
  public void show() {
    System.out.println("This is nike shirt.");
  }

  @Override
  public void add(Shirt shirt) {
    throw new UnsupportedOperationException();
  }

  @Override
  public void remove(Shirt shirt) {
    throw new UnsupportedOperationException();
  }
}

public class ShirtStore implements  Shirt {
  private List<Shirt> shirts;


  public ShirtStore() {
    this.shirts = new ArrayList<>();
  }

  @Override
  public void show() {
    for (Shirt shirt : shirts) {
      shirt.show();
    }
  }

  @Override
  public void add(Shirt shirt) {
    this.shirts.add(shirt);
  }

  @Override
  public void remove(Shirt shirt) {
    this.shirts.remove(shirt);
  }
}

public class Test {
  public static void main(String[] args) {
    Shirt adidasShirt = new AdidasShirt();
    Shirt nikeShirt = new NikeShirt();
    Shirt shirtStore = new ShirtStore();
    shirtStore.add(adidasShirt);
    shirtStore.add(nikeShirt);

    Shirt superStore = new ShirtStore();
    superStore.add(nikeShirt);
    superStore.add(adidasShirt);
    superStore.add(shirtStore);

    superStore.show();
  }
}

```

