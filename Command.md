### Command(命令模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/command.png)

#### 意图

将请求封装成对象，可以对请求进行排队，记录以及撤销

#### 使用场景
- 不知道请求的接受者的信息时
- 请求支持撤销
- 支持修改日志以及事务

#### 优缺点
- 将操作的请求者以及接收者解耦
- 可以将多个命令装配成一个组合命令
- 在无需改变已有的类下，增加新的Command

#### 想法
Command模式主要是将请求封装成一个对象，实现操作的请求者和接收者的解耦，可以对命令对象进行排队，记录以及撤销。

#### 示例代码
buy衣服的行为，只有顾客知道买什么样的衣服去什么店里买，知道行为的接收者是谁。
```
public interface Shirt {
  void show();
}

public class AdidasShirt implements Shirt {
  @Override
  public void show() {
    System.out.println("This is adidas shirt.");
  }
}

public class NikeShirt implements Shirt{
  @Override
  public void show() {
    System.out.println("This is nike shirt.");
  }
}

public interface Shop {
   Shirt sell();
}

public class NikeShop  implements Shop{
  @Override
  public Shirt sell() {
    return new NikeShirt();
  }
}

public class AdidasShop implements Shop {
  @Override
  public Shirt sell() {
    return new AdidasShirt();
  }
}

public interface BuyShirt {
  Shirt buy();
}

public class BuyNikeShirt implements BuyShirt {
  private NikeShop nikeShop;

  public BuyNikeShirt(NikeShop nikeShop) {
    this.nikeShop = nikeShop;
  }

  @Override
  public Shirt buy() {
    return this.nikeShop.sell();
  }
}

public class BuyAdidasShirt implements BuyShirt {
  private AdidasShop adidasShop;

  public BuyAdidasShirt(AdidasShop adidasShop) {
    this.adidasShop = adidasShop;
  }

  @Override
  public Shirt buy() {
    return this.adidasShop.sell();
  }
}

public class Guest {
  public static void main(String[] args) {
    Shop nikeShop = new NikeShop();
    Shop adidasShop = new AdidasShop();
    BuyShirt buyAdidasShirt = new BuyAdidasShirt(adidasShop);
    BuyShirt buyNikeShirt = new BuyNikeShirt(nikeShop);
    buyAdidasShirt.buy().show();
    buyNikeShirt.buy().show();
  }
}

```
