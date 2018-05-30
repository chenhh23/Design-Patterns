
### Builder(生成器模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/builder.png)

#### 意图

一般用于复杂对象的构建, 将复杂对象的构建过程与它的最终形态分离开来

#### 使用场景

- 复杂对象构建过程中可以允许有不同阶段的展示形态

- 复杂对象的表示与其构建，装配过程能够分离开来, Builder关注于对象的构建和装配, 而Director提供创建的算法以及创建的参数。

- 同一套创建的算法以及创建参数可以应用于不同的Builder创建不同的复杂对象

#### 优缺点

- 对象的表示和其构造分离, 意味着对于同一个对象的表示和它的创建算法可以复用多个Builder实现不同的构建和装配

- 对于构造对象的过程能够更加精细的进行控制，但也需要在获取构造好的产品时进行控制，避免返回一些不允许的残次品

- 因为Builder装配的产品差别比较大，没有一个公共的抽象Product类, 所以对象的表示需要依赖到Builder返回的具体的Product

#### 想法

Builder模式的作用不仅仅局限于我们平时所用到的用Builder去构造一个Model, 这仅仅是使用到了它控制精细构建过程的一小部分作用。关键点还在于:

- 复杂对象的创建算法与它的构建、装配过程分离，从而达到创建算法复用多个Builder，构建不同的复杂对象

#### 示例代码

根据用户Name和邮箱 构造欢迎邮件和回复邮件。

```
// 邮件构造器接口
public interface EmailBuilder {
  void buildName(String name);

  void buildEmail(String email);
}

// 欢迎邮件
public class WelcomeEmailBuilder implements EmailBuilder {
  private String body = "Hi, %s Welcome to join us. your email address: %s";

  private String name;
  private String email;

  @Override
  public void buildName(String name) {
    this.name = name;
  }

  @Override
  public void buildEmail(String email) {
    this.email = email;
  }

  public String build() {
    Objects.requireNonNull(name);
    Objects.requireNonNull(email);
    return String.format(body, name, email);
  }
}

// 回复邮件
public class ReplyEmailBuilder implements EmailBuilder {
  private String body = "%s, Thanks to your reply!";

  private String name;

  @Override
  public void buildName(String name) {
    this.name = name;
  }

  @Override
  public void buildEmail(String email) {

  }

  public String build() {
    Objects.requireNonNull(name);
    return String.format(body, name);
  }
}

// builder构造邮件
public class Person {
  private String name;
  private String email;

  public Person(String name, String email) {
    this.name = name;
    this.email = email;
  }

  void buildEmail(EmailBuilder builder) {
    builder.buildEmail(email);
    builder.buildName(name);
  }

  public static void main(String args[]) {
    Person person = new Person("huahuan", "huahuan.chen@renren-inc.com");
    WelcomeEmailBuilder welcomeEmailBuilder = new WelcomeEmailBuilder();
    ReplyEmailBuilder replyEmailBuilder = new ReplyEmailBuilder();
    person.buildEmail(welcomeEmailBuilder);
    person.buildEmail(replyEmailBuilder);
    System.out.println(welcomeEmailBuilder.build());
    System.out.println(replyEmailBuilder.build());
  }
}
```
