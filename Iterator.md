### Iterator(迭代器模式)

![image](https://github.com/chenhh23/Design-Patterns/blob/master/design-picture/iterator.png)

#### 意图

迭代器模式提供一种隐藏对象内部细节地去遍历一个聚合对象内部元素的方法。

#### Key
- Iterator 迭代器定义访问和遍历元素的接口
- ConcreteIterator 具体迭代器实现迭代器接口
- Aggregate 聚合定义创建相应迭代器对象的接口
- ConcreteAggregate 具体聚合实现创建相应迭代器的接口

#### 使用场景
- 不暴露对象内部细节地遍历一个聚合对象的元素
- 能够容易地实现对聚合对象的多种遍历
- 为遍历不同的聚合接口提供一个统一的接口

#### 优缺点
- 可以支持不同方式遍历一个聚合，可以通过不同的迭代器实例来支持新的遍历
- 简化聚合的接口，聚合接口本身不需要关心自身元素遍历的实现
- 在同一个聚合上可以有多个遍历
- 对于比较简单的遍历（像数组或者有序列表），使用迭代器方式遍历较为繁琐，像ArrayList，我们宁可愿意使用for循环和get方法来遍历集合

#### 想法
Iterator模式主要是能够提供不暴露内部细节地遍历聚合元素的一种方式，可以分离聚合内部和遍历的实现，从而能够以不同的方式来遍历一个聚合。迭代器能够遍历聚合内部元素主要是通过，外部迭代器通过持有聚合的引用，内部迭代器作为聚合的内部类能够访问到外部类的成员，这种方式里实现的。

#### 示例代码
针对一个聚合实现一个迭代器
```
public interface Iterable<T> {
  Iterator<T> iterator();
}


public interface Aggregate<T> extends Iterable<T> {
  void add(T object);

  void remove(int index);

  T get(int index);
}

public class ConcreteAggregate<T> implements Aggregate<T> {
  private List<T> list;

  public ConcreteAggregate() {
    this.list = new ArrayList<>();
  }

  @Override
  public void add(T object) {
    list.add(object);
  }

  @Override
  public void remove(int index) {
    list.remove(index);
  }

  @Override
  public T get(int index) {
    return list.get(index);
  }

  @Override
  public Iterator<T> iterator() {
    return new ConcreteIterator(list);
  }
}

public interface Iterator<T> {
  boolean hasNext();

  T next();
}

public class ConcreteIterator<T> implements Iterator {
  private int cursor;
  private List<T> list;

  public ConcreteIterator(List<T> list) {
    cursor = 0;
    this.list = list;
  }

  @Override
  public boolean hasNext() {
    return cursor < list.size();
  }

  @Override
  public T next() {
    return list.get(cursor++);
  }
}

public class Test {
  public static void main(String args[]) {
    Aggregate<String> aggregate = new ConcreteAggregate();
    aggregate.add("A");
    aggregate.add("B");
    aggregate.add("C");
    Iterator iterator = aggregate.iterator();
    while (iterator.hasNext()) {
      System.out.println(iterator.next());
    }
  }
}

```

