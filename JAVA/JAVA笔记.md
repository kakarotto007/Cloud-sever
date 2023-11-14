# 值传递和引用传递

可以改变：

```java
public class Person {
    private String name;
    // 省略构造函数、Getter&Setter方法

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

public static void main(String[] args) {
    Person xiaoZhang = new Person();
    xiaoZhang.setName("小张");
    Person xiaoLi = new Person();
    xiaoLi.setName("小李");
    swap(xiaoZhang, xiaoLi);
    System.out.println("xiaoZhang: " + xiaoZhang.getName());
    System.out.println("xiaoLi: " + xiaoLi.getName());
}

public static void swap(Person person1, Person person2) {
    String tempName = person1.getName();
    person1.setName(person2.getName());
    person2.setName(tempName);
}
```

不可以改变：

```java
public class Person {
    private String name;
   // 省略构造函数、Getter&Setter方法
}

public static void main(String[] args) {
    Person xiaoZhang = new Person("小张");
    Person xiaoLi = new Person("小李");
    swap(xiaoZhang, xiaoLi);
    System.out.println("xiaoZhang:" + xiaoZhang.getName());
    System.out.println("xiaoLi:" + xiaoLi.getName());
}

public static void swap(Person person1, Person person2) {
    Person temp = person1;
    person1 = person2;
    person2 = temp;
    System.out.println("person1:" + person1.getName());
    System.out.println("person2:" + person2.getName());
}

```

# 浅拷贝和深拷贝

基本类型也是”深拷贝“

浅拷贝是一个变的话都要变

深拷贝是申请出一块内存复制出一块内容，改变的话，初始的不会改变。

# 线程阻塞状态

1. 等待阻塞：当线程调用了某个对象的**wait()**方法时，线程将进入等待阻塞状态，直到其他线程调用相同对象的notify()或notifyAll()方法来唤醒它。
2. 同步阻塞：当线程试图进入一个**synchronized**关键字修饰的代码块时，如果该代码块已经被其他线程占用，线程将进入同步阻塞状态，直到获取到锁才能继续执行。
3. 睡眠阻塞：当线程调用Thread类的**sleep()**方法时，线程将进入睡眠阻塞状态，暂时停止执行一段指定的时间。
4. I/O阻塞：当线程进行输入输出操作时，如果数据还未准备好或者无法立即读写，线程将进入I/O阻塞状态，直到数据就绪或者操作完成。
5. 条件阻塞：当线程等待某个条件满足时，可以调用Lock接口的Condition对象的await()方法，线程将进入条件阻塞状态，直到其他线程通过signal()或signalAll()方法发出信号来唤醒它。

# 线程的六个状态

1. new状态，还没有调用start（）方法
2. runnable状态：表示当前线程正在运行中，处于 RUNNABLE 状态的线程在 Java 虚拟机中运行，也有可能在等待 CPU 分配资源。包含了操作系统线程的**ready**和**running**两个状态

> 操作系统的线程状态：
>
> - 就绪状态(ready)：线程正在等待使用 CPU，经调度程序调用之后进入 running 状态。
> - 执行状态(running)：线程正在使用 CPU。
> - 等待状态(waiting): 线程经过等待事件的调用或者正在等待其他资源（如 I/O）

3. blocked状态：阻塞状态，假设你是线程 t2，你前面的那个人是线程 t1。此时 t1 占有了锁（食堂唯一的窗口），t2 正在等待锁的释放，所以此时 t2 就处于 BLOCKED 状态。
4. waiting状态：处于等待状态的线程变成 **runnable**状态需要其他线程唤醒。调用下面这 3 个方法会使线程进入**等待状态**：
   - `Object.wait()`：使当前线程处于等待状态直到另一个线程唤醒它；
   - `Thread.join()`：等待线程执行完毕，底层调用的是 Object 的 wait 方法；
   - `LockSupport.park()`：除非获得调用许可，否则禁用当前线程进行线程调度。[LockSupportopen in new window](https://javabetter.cn/thread/LockSupport.html) 我们在后面会细讲。

5. timed_waiting状态：超时等待状态。线程等待一个具体的时间，时间到后会被自动唤醒。用sleep，wait等
6. terminated状态：终止了，此时线程已经执行完毕。

![img](https://raw.githubusercontent.com/kakarotto007/final/master/thread-state-and-method-20230829143200.png)

# string和char[]相互转换

string -> char[] : string. tocharArray

char[] -> string : new string( char[] )