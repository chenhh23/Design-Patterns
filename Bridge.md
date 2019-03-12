### Bridge(桥接模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/bridge.png)

#### 意图

将抽象部分和实现部分分离，相互之间的变化独立，抽象和它的实现部分分别放在独立的类层次结构中。

#### 使用场景
- 不想要接口和实现有固定的绑定关系，接口抽象部分不依赖于实现部分
- 不同接口和实现的子类能够自由的进行组合
- 一般来说implementor接口定义的是基本操作接口，而Abstraction定义的是基于基本操作的高级接口
- 可以在多个对象当中共享同一个实现

#### 优缺点
- 接口抽象和实现部分分离，可以在运行时动态的进行改变，系统的高层只需要知道Abstraction和Implementor就可以了
- 实现的细节对于客户来说是透明的。

#### 想法
Bridge模式主要点是能够将抽象部分和实现部分分离，抽象部分的子类持有对实现部分的引用，以此来实现抽象部分的功能， 一般来说，抽象部分和实现部分的接口不一定需要一致，implementor接口定义的是基本操作接口，而实现部分更可能的是基于基本接口建立的高级操作。至于抽象需要引用什么样的实现部分，可以在构造器中根据参数选择，也可以通过委托给工厂类返回。

#### 示例代码
通过提供不同品牌不同价格ShirtImpl，来输出不同的信息
```
public interface Shirt {
    String print();
}

public class RedShirt implements Shirt {
    private ShirtImpl shirt;
    public RedShirt(ShirtImpl shirt) {
        this.shirt=shirt;
    }

    public String print() {
        return String.format("color:%s,brand:%s,price:%s",shirt.showColor(),
                shirt.showBrand(),shirt.showPrice());
    }
}

public interface ShirtImpl {
    String showColor();
    String showBrand();
    String showPrice();
}

public class NikeShirtImpl implements ShirtImpl {
    public String showColor() {
        return "red";
    }

    public String showBrand() {
        return "nike";
    }

    public String showPrice() {
        return "100$";
    }
}

public class AdidasShirtImpl implements ShirtImpl {
    public String showColor() {
        return "red";
    }

    public String showBrand() {
        return "adidas";
    }

    public String showPrice() {
        return "110$";
    }
}

public class Person {
    public void wear(Shirt shirt) {
        System.out.println(shirt.print());
    }

    public static void main(String[] args) {
        ShirtImpl adidasShirtImpl = new AdidasShirtImpl();
        ShirtImpl nikeShirtImpl = new NikeShirtImpl();

        Shirt adidasShirt = new RedShirt(adidasShirtImpl);
        Shirt nikeShirt = new RedShirt(nikeShirtImpl);

        Person person = new Person();
        person.wear(adidasShirt);
        person.wear(nikeShirt);
    }
}

```
