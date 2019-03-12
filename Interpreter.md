### Interpreter(解释器模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/interpreter.png)

#### 意图

解释器模式是一种类行为模式，可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子

#### Key
- Expression 声明一个所有的具体表达式角色都需要实现的抽象接口。这个接口主要是一个interpret()方法，称做解释操作。
- TerminalExpression 实现了抽象表达式角色所要求的接口，主要是一个interpret()方法；文法中的每一个终结符都有一个具体终结表达式与之相对应。比如有一个简单的公式R=R1+R2，在里面R1和R2就是终结符，对应的解析R1和R2的解释器就是终结符表达式。
- NonterminalExpression 文法中的每一条规则都需要一个具体的非终结符表达式，非终结符表达式一般是文法中的运算符或者其他关键字，比如公式R=R1+R2中，“+"就是非终结符，解析“+”的解释器就是一个非终结符表达式。

#### 使用场景
- 语言需要解释运行，语言中的句子能够表示成一个抽象语法树
- 文法简单容易表示，复杂的文法会导致表达式类层次结构庞大难以管理
- 性能和效率不是关键，因为解释和构建抽象语法树会导致效率和性能的损耗

#### 优缺点
- 易于改变和拓展文法， 解释器模式使用类来表达文法规则，可以通过继承类来拓展修改文法
- 文法容易实现 抽象语法树里面各个节点实现大体上相似
- 只适用于相对简单的文法，对于复杂的文法无能为力，会导致产生复杂的类层次结构，难以管理维护

#### 想法
Interpreter模式主要是能够提供语言的解释方法，利用类来表示文法规则，根据语言中的句子构建抽象语法树进行解释，一般来说，只适用于构建简单的文法规则，对于复杂的文法，会产生复杂的类层次结构，难以管理，而且也会导致低效率的问题

#### 示例代码
针对 + - * / 文法的解释器模式
`// (d - (a + b)) * c / b = (400 - (100 + 200)) * 300 / 200 = 150`
```
public interface Expression {
  int interpreter(Context context);
}

public class Add implements Expression {
  private Expression left;
  private Expression right;

  public Add(Expression left, Expression right) {
    this.left = left;
    this.right = right;
  }

  @Override
  public int interpreter(Context context) {
    return left.interpreter(context) + right.interpreter(context);
  }

}

public class Sub implements Expression {
  private Expression left;
  private Expression right;

  public Sub(Expression left, Expression right) {
    this.left = left;
    this.right = right;
  }

  @Override
  public int interpreter(Context context) {
    return left.interpreter(context) - right.interpreter(context);
  }
}

public class Mul implements Expression {
  private Expression left;
  private Expression right;

  public Mul(Expression left, Expression right) {
    this.left = left;
    this.right = right;
  }

  @Override
  public int interpreter(Context context) {
    return left.interpreter(context) * right.interpreter(context);
  }
}

public class Div implements Expression {
  private Expression left;
  private Expression right;

  public Div(Expression left, Expression right) {
    this.left = left;
    this.right = right;
  }

  @Override
  public int interpreter(Context context) {
    return left.interpreter(context) / right.interpreter(context);
  }
}

public class Variable implements Expression {

  @Override
  public int interpreter(Context context) {
    return context.get(this);
  }
}

public class Context {
  private static final Map<Expression, Integer> MAP = new HashMap<>();

  public void put(Expression variable, int num) {
    MAP.put(variable, num);
  }

  public int get(Expression variable) {
    return MAP.get(variable);
  }
}

public class Client {
  public static void main(String args[]) {
    Expression a = new Variable();
    Expression b = new Variable();
    Expression c = new Variable();
    Expression d = new Variable();
    Context context = new Context();
    context.put(a, 100);
    context.put(b, 200);
    context.put(c, 300);
    context.put(d, 400);
    // (d - (a + b)) * c / b = (400 - (100 + 200)) * 300 / 200 = 150
    System.out.println(new Div(new Mul(new Sub(d, new Add(a, b)), c), b).interpreter(context));
  }
}


```
