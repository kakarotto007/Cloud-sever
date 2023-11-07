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