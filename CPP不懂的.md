# 常量指针和指针常量

## 常量指针

指针是常量 ， 即指针的指向不能改变

`int *const tmp =&i`

## 指针常量

指针的指向为常量，即指针指向的值不能改变

`const int* tmp = &i`

# 类（class）和结构（struct）的区别

**概念**：class和[struct](https://so.csdn.net/so/search?q=struct&spm=1001.2101.3001.7020)的语法基本相同，从声明到使用，都很相似，但是struct的约束要比class多，理论上，struct能做到的class都能做到，但class能做到的stuct却不一定做的到。

**类型**：struct是值类型，class是引用类型，因此它们具有所有值类型和引用类型之间的差异。
**效率**：由于栈的执行效率要比堆的执行效率高，但是栈资源却很有限，不适合处理逻辑复杂的大对象，因此struct常用来处理作为基类型对待的小对象，而class来处理某个商业逻辑

1.默认的继承访问权。**class默认的是private,strcut默认的是public。**
2.默认访问权限：struct作为数据结构的实现体，它默认的数据访问控制是public的，而class作为对象的实现体，它默认的成员变量访问控制是private的。
3.“class”这个关键字还用于定义模板参数，就像“typename”。但关建字“struct”不用于定义模板参数



- 结构（struct）是实值类型（Value Types），而类（class）则是引用类型（Reference Types）。
- 结构（struct）使用栈存储（Stack Allocation），而类（class）使用堆存储（Heap Allocation）。

# 友元（friend）函数

类可以允许其他类或函数访问它的非公有成员，方法是使用关键字`friend`将其他类或函数声明为它的友元。

通常情况下，最好在类定义开始或结束前的位置**集中声明友元**。

```Cpp
class Sales_data
{
    // friend declarations for nonmember Sales_data operations added
    friend Sales_data add(const Sales_data&, const Sales_data&);
    friend std::istream &read(std::istream&, Sales_data&);
    friend std::ostream &print(std::ostream&, const Sales_data&);

    // other members and access specifiers as before
public:
    Sales_data() = default;
    Sales_data(const std::string &s, unsigned n, double p):
    bookNo(s), units_sold(n), revenue(p*n) { }
    Sales_data(const std::string &s): bookNo(s) { }
    Sales_data(std::istream&);
    std::string isbn() const { return bookNo; }
    Sales_data &combine(const Sales_data&);

private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
// declarations for nonmember parts of the Sales_data interface
Sales_data add(const Sales_data&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
```

友元声明仅仅指定了访问权限，而并非一个通常意义上的函数声明。如果希望类的用户能调用某个友元函数，**就必须在友元声明之外再专门对函数进行一次声明**（部分编译器没有该限制）。

为了使友元对类的用户可见，通常会把**友元的声明（类的外部）与类本身**放在同一个**头文件**中。

## 友元再探（Friendship Revisited）

除了普通函数，类还可以把其他类或其他类的成员函数声明为友元。友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

```cpp
class Screen
{
    // Window_mgr members can access the private parts of class Screen
    friend class Window_mgr;
    // ... rest of the Screen class
};
```

把其他类的成员函数声明为友元时，必须明确指定该函数所属的类名。

```cpp
class Screen
{
    // Window_mgr::clear must have been declared before class Screen
    friend void Window_mgr::clear(ScreenIndex);
    // ... rest of the Screen class
};
```



# 返回*this的成员函数

`const`成员函数如果以引用形式返回`*this`，则返回类型是常量引用。

通过区分成员函数是否为`const`的，可以对其进行重载。在常量对象上只能调用`const`版本的函数；在非常量对象上，尽管两个版本都能调用，但会选择**非常量版本**。

> ## 不会

# 函数重载

> # 仅仅返回类型不同不足以成为函数的重载。

函数重载是指在同一个作用域内定义多个具有相同名称但参数列表不同的函数。这些函数可以具有不同的返回类型，**但不能只有返回类型不同而参数列表相同。**

函数重载的规则如下：

1. 函数名称必须相同，但参数列表必须不同。参数列表可以包括**参数的类型、参数的数量或参数的顺序。**
2. 返回类型不被视为函数重载的一部分。因此，**不能仅通过返回类型的不同来重载函数。**
3. 函数重载可以在同一个类中定义，也可以在不同的作用域中定义（例如，全局作用域或命名空间）。
4. 在函数调用时，编译器根据实际传递的参数类型和数量来选择最佳匹配的函数版本。
5. 编译器在候选函数集合中寻找与实际参数最匹配的函数，如果找到了一个完全匹配的函数，将调用该函数。
6. 如果没有找到完全匹配的函数，编译器会尝试进行隐式类型转换或使用默认参数匹配来寻找最佳匹配的函数。
7. 如果找到多个匹配的函数，但没有一个函数是明显的最佳匹配，将导致函数重载冲突，编译器将报错。



使用const声明的函数可以与不使用const声明的函数形成重载函数。例如，如果有一个函数可以处理非常量int参数，并且有一个函数可以处理常量int参数，则可以使用以下代码实现：

```cpp
void process(int x) { ... }
void process(const int x) { ... }
```

# new对象与直接声明对象区别

在C++中，创建对象有两种主要方式：使用`new`运算符动态分配对象并返回指向对象的指针，或者直接在**栈**上声明对象。

1. 使用new运算符创建对象：

   ```cpp
   MyClass* obj = new MyClass();
   这种方式使用动态内存分配，在堆上创建对象，并返回指向对象的指针。由于对象是在堆上分配的，它们的生命周期不受限于当前作用域。因此，对象将一直存在，直到使用`delete`运算符显式释放它们，或者在动态对象的容器（如`std::vector`）中使用时，容器本身被销毁。
   
   优点：
   - 对象的生命周期可以由开发人员显式控制。
   - 对象的大小可以动态确定，不受栈大小的限制。
   
   缺点：
   - 需要手动管理内存，确保及时释放对象，避免内存泄漏。
   - 容易出现内存泄漏和悬空指针等问题，需要小心处理。

2. 直接声明对象

   ```cpp
   MyClass obj;
   这种方式在栈上分配对象，对象的生命周期受限于当前作用域。当声明的对象超出作用域时，对象会自动被销毁，释放占用的内存。
   
   优点：
   - 无需手动管理内存，对象的生命周期自动管理。
   - 代码更简洁，不需要显式调用`delete`来释放对象。
   
   缺点：
   - 对象的大小需要在编译时确定，受栈大小的限制。
   - 对象的生命周期受限于作用域，不适用于需要在多个作用域中共享的对象。

# constexpr

在C++中，`constexpr`是一个关键字，用于指示编译器在编译时求值并将结果视为常量表达式。它可以用于声明变量、函数和构造函数，以及在编译时执行计算和初始化操作。

类的构造函数：

```cpp
class Circle {
private:
    constexpr static double PI = 3.14159;
    double radius;

public:
    constexpr Circle(double r) : radius(r) {}

    constexpr double getArea() const {
        return PI * radius * radius;
    }
};

constexpr Circle c(5.0);  // 在编译时初始化Circle对象
constexpr double area = c.getArea();  // 在编译时求值
```

在这个例子中，`Circle`类的构造函数被声明为`constexpr`，它允许在编译时初始化`Circle`对象。`getArea`函数也被声明为`constexpr`，允许在编译时求得圆的面积。

需要注意的是，`constexpr`要求表达式在编译时就能得到求值结果。因此，它的使用有一些限制，例如不能包含动态分配的内存、不确定的运行时函数调用等。但是，使用`constexpr`可以提高编译时的性能和代码的可读性，同时还可以在一些需要常量表达式的上下文中使用，例如模板参数、数组大小等.

# 基础的IO库

- `istream`：输入流类型，提供输入操作。
- `ostream`：输出流类型，提供输出操作。
- `cin`：`istream`对象，从标准输入读取数据。
- `cout`：`ostream`对象，向标准输出写入数据。
- `cerr`：`ostream`对象，向标准错误写入数据。
- `>>`运算符：从`istream`对象读取输入数据。
- `<<`运算符：向`ostream`对象写入输出数据。
- `getline`函数：从`istream`对象读取一行数据，写入`string`对象。

# IO对象不能进行拷贝或赋值

如果需要多个IO对象引用同一流，可以使用引用或指针来实现

```cpp
    std::ostream& output = std::cout;
    std::istream& input = std::cin;

    output << "Enter a number: ";
    int num;
    input >> num;
```

# fstream的文件模式

1. std::ios::in：以输入模式打开文件，用于读取文件内容。
2. std::ios::out：以输出模式打开文件，用于写入文件内容。如果文件已存在，原有内容会被清除。
3. std::ios::app：在输出模式下打开文件，并定位到文件末尾，用于将新数据追加到文件末尾。
4. std::ios::ate：在输入或输出模式下打开文件，并定位到文件末尾。可以在文件中进行读取和写入操作。
5. std::ios::binary：以二进制模式打开文件，用于处理二进制数据。
6. std::ios::trunc：在输出模式下打开文件，如果文件已存在，则清除原有内容。

`file.open("example.txt", std::ios::out | std::ios::trunc);`

# 深拷贝和浅拷贝

浅拷贝 (shallow copy) 只是对指针的拷贝, 拷贝够两个指针指向同一个内存空间. 深拷贝 (deep copy) 不但对指针进行拷贝, 而且对指针指向的内容进行拷贝. 经过深拷贝后的指针是指向两个不同地址的指针.

# 顺序容器

| 类型           | 特性                                                         |
| -------------- | ------------------------------------------------------------ |
| `vector`       | 可变大小数组。支持快速随机访问。在尾部之外的位置插入/删除元素可能很慢 |
| `deque`        | 双端队列。支持快速随机访问。在头尾位置插入/删除速度很快      |
| `list`         | 双向链表。只支持双向顺序访问。在任何位置插入/删除速度都很快  |
| `forward_list` | 单向链表。只支持单向顺序访问。在任何位置插入/删除速度都很快  |
| `array`        | 固定大小数组。支持快速随机访问。不能添加/删除元素            |
| `string`       | 类似`vector`，但用于保存字符。支持快速随机访问。在尾部插入/删除速度很快 |

容器选择原则：

- 除非有合适的理由选择其他容器，否则应该使用`vector`。
- 如果程序有很多小的元素，且空间的额外开销很重要，则不要使用`list`或`forward_list`。
- 如果程序要求随机访问容器元素，则应该使用`vector`或`deque`。
- 如果程序需要在容器头尾位置插入/删除元素，但不会在中间位置操作，则应该使用`deque`。
- 如果程序只有在读取输入时才需要在容器中间位置插入元素，之后需要随机访问元素。则：
  - 先确定是否真的需要在容器中间位置插入元素。当处理输入数据时，可以先向`vector`追加数据，再调用标准库的`sort`函数重排元素，从而避免在中间位置添加元素。
  - 如果必须在中间位置插入元素，可以在输入阶段使用`list`。输入完成后将`list`中的内容拷贝到`vector`中。
- 不确定应该使用哪种容器时，可以先只使用`vector`和`list`的公共操作：使用迭代器，不使用下标操作，避免随机访问。这样在必要时选择`vector`或`list`都很方便。

# 迭代器iterator

这两个迭代器通常被称为`begin`和`end`，分别指向同一个容器中的元素或尾后地址。`end`迭代器不会指向范围中的最后一个元素，而是指向尾元素之后的位置。

假定`begin`和`end`构成一个合法的迭代器范围，则：

- 如果`begin`等于`end`，则范围为空。
- 如果`begin`不等于`end`，则范围内至少包含一个元素，且`begin`指向该范围内的第一个元素。
- 可以递增`begin`若干次，令`begin`等于`end`。
- 

# makefile

大家第一次写 hello world 程序的时候一定都记得，在编辑完 `helloworld.c` 之后，需要用 `gcc` 编译生成可执行文件，然后再执行（如果你不理解前面这段话，请先自行谷歌 *gcc 编译* 并理解相关内容）。但如果你的项目由成百上千个 C 源文件组成，并且星罗棋布在各个子目录下，你该如何将它们编译链接到一起呢？假如你的项目编译一次需要半个小时（大型项目相当常见），而你只修改了一个分号，是不是还需要再等半个小时呢？

这时候 GNU Make 就闪亮登场了，它能让你在一个脚本里（即所谓的 `Makefile`）定义整个编译流程以及各个目标文件与源文件之间的依赖关系，并且只重新编译你的修改会影响到的部分，从而降低编译的时间。

# string::size_type

`td::string::size_type` 是 `std::string` 类的一个无符号整数类型，用于表示字符串的**大小或索引**。它是一个与特定实现相关的类型，通常被定义为无符号整数类型，足以容纳字符串的最大可能大小

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "Hello, World!";
    std::string::size_type size = str.size();

    std::cout << "Size of the string: " << size << std::endl;

    for (std::string::size_type i = 0; i < size; ++i) {
        std::cout << "Character at index " << i << ": " << str[i] << std::endl;
    }

    return 0;
}
```

# sizeof获取数组长度

```cpp
int a1[] = { 0,1,2,3,4,5,6,7,8,9 };
int a2[sizeof(a1) / sizeof(*a1)];     // a2 has the same size as a1
```

# 数组指针与指针数组

## 指针数组

`int*ptr[10]`,`[]` 的优先级高于 `*` ，所以这是一个数组，而 `*`修饰数组，所以是指针数组，数组的元素是整型的指针。

## 数组指针

**int (\*a)[3]**：同样的方式，首先括号的优先级最高，所以 ***a** 是指针，而 **[]** 修饰 ***a** ，所以是数组指针，一个指向 3 个元素的一维数组指针

```cpp
typedef int arr[3]; 
int main() {     
   arr b = {1, 2, 3};     
   int (*a)[3] = &b;     
   arr *c = a;     
   for (int i = 0; i < 3; ++i) {         
      printf("%d\n", (*a)[i]);     
      
   } 
}
```

arr 表示 int[3]	

# typedef用法

下面是一些常见的 `typedef` 用法示例：

1. 别名定义：

```cpp
typedef int myInt;  // 创建一个名为 myInt 的别名，表示 int 类型
myInt num = 10;  // 使用 myInt 别名声明变量
```

1. 结构体别名：

```cpp
typedef struct {
    int x;
    int y;
} Point;  // 创建一个名为 Point 的别名，表示结构体类型
Point p1 = {1, 2};  // 使用 Point 别名声明结构体变量
```

1. 函数指针别名：

```cpp
typedef int (*MathFunc)(int, int);  // 创建一个名为 MathFunc 的别名，表示函数指针类型
int add(int a, int b) { return a + b; }
MathFunc funcPtr = add;  // 使用 MathFunc 别名声明函数指针变量
int result = funcPtr(2, 3);  // 调用函数指针
```

1. 枚举别名：

```cpp
typedef enum {
    RED,
    GREEN,
    BLUE
} Color;  // 创建一个名为 Color 的别名，表示枚举类型
Color c = RED;  // 使用 Color 别名声明枚举变量
```

1. 复杂类型别名：

```cpp
typedef int (*(*ComplexFunc)(int))(int);  // 创建一个名为 ComplexFunc 的别名，表示复杂类型的函数指针
int innerFunc(int x) { return x * x; }
int (*outerFunc(int y))(int) {
    int (*funcPtr)(int) = innerFunc;
    return funcPtr;
}
ComplexFunc complexPtr = outerFunc;  // 使用 ComplexFunc 别名声明复杂类型的函数指针变量
int result = complexPtr(3)(4);  // 调用复杂类型的函数指针
```

上述示例展示了 `typedef` 在不同场景下的用法。它可以用于简化复杂类型的声明，提高代码的可读性，并允许我们使用更具描述性的名称来表示类型。

# 数组名

在C++中，数组名表示数组的首元素的地址。当您在代码中使用数组名时，它通常会被解释为指向数组的第一个元素的指针。	

您可以使用数组名来访问数组的元素。例如，`arr[0]` 表示数组的第一个元素，`arr[1]` 表示数组的第二个元素，以此类推。

此外，数组名还可以隐式转换为指向数组的第一个元素的指针。例如，如果将数组名 `arr` 分配给指针变量，它将自动转换为指向数组的第一个元素的指针，如下所示：

```
int* ptr = arr;  // 将数组名赋值给指针变量
```

在这里，`ptr` 成为指向数组 `arr` 的第一个元素的指针。	

# back_inserter函数

C++标准库中的`back_inserter`函数是一个辅助函数，用于创建一个插入迭代器，可将元素添加到容器的末尾。

`back_inserter`函数定义在`<iterator>`头文件中，并且通常与算法函数一起使用，用于向容器中添加元素。它接受一个容器作为参数，并返回一个插入迭代器，该迭代器可以用于将元素添加到容器的末尾。

使用`back_inserter`函数的一般用法如下：    

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <iterator>
int main() {
    std::vector<int> vec;
// 使用 back_inserter 创建插入迭代器
auto it = std::back_inserter(vec);

// 通过插入迭代器向容器中添加元素
*it = 42; // vec 现在包含一个值为 42 的元素
*it = 10; // vec 现在包含两个元素，分别为 42 和 10

// 使用算法函数向容器中添加多个元素
std::fill_n(std::back_inserter(vec), 5, 0); // vec 现在包含 7 个元素，前两个为 42 和 10，其余为 0

// 输出容器中的所有元素
for (const auto& element : vec) {
    std::cout << element << " ";
}
std::cout << std::endl;

return 0;
}
```
在上述示例中，`back_inserter`函数用于创建一个插入迭代器`it`，然后可以通过`*it`的方式将元素添加到容器`vec`的末尾。此外，还可以将`back_inserter`函数作为算法函数的参数，如`std::fill_n`函数中所示，用于将一定数量的元素添加到容器中。

通过使用`back_inserter`函数，可以方便地在不预先分配容器空间的情况下，逐个向容器中添加元素。

# 内置类型

在C++中，内置类型（Built-in Types）是指由语言本身提供的基本数据类型，这些类型不依赖于任何库或用户自定义类型。C++的内置类型包括以下几种：

1. 基本整数类型（Basic Integer Types）：包括 `int`、`unsigned int`、`short`、`unsigned short`、`long`、`unsigned long`、`long long`、`unsigned long long`等。
2. 基本浮点类型（Basic Floating-Point Types）：包括 `float`、`double`和 `long double`。
3. 字符类型（Character Types）：包括 `char`、`signed char`、`unsigned char`和 `wchar_t`。
4. 布尔类型（Boolean Type）：`bool`，只有两个取值：`true`和`false`。
5. 空类型（Void Type）：`void`，表示没有类型。

# 组合类型

C++中的组合类型包括以下几种：

1. 数组类型（Array Types）：由相同类型的元素组成的固定大小的序列。例如，`int numbers[10]` 声明了一个包含10个整数的数组。
2. 结构体类型（Structure Types）：由多个不同类型的成员组成的自定义类型。结构体中的成员可以是内置类型或其他组合类型。例如，定义一个表示学生的结构体：`struct Student { string name; int age; float gpa; };`。
3. 联合类型（Union Types）：与结构体类似，但联合类型的成员共享同一块内存空间，只能同时存储其中一个成员。联合类型的大小等于其最大成员的大小。例如，定义一个表示图形的联合类型：`union Shape { int width; int height; float radius; };`。
4. 枚举类型（Enumeration Types）：用于定义一组具名的整数常量。枚举类型的取值为预定义的枚举常量。例如，定义一个表示颜色的枚举类型：`enum Color { RED, GREEN, BLUE };`。

# Cin 、 getline

`getline`函数是用于从输入流中读取一行数据的函数。它可以读取包含空格在内的整行数据，并将其存储到字符串变量中。`getline`函数的使用方法如下：

```cpp
#include <iostream>
#include <string>

int main() {
    std::string line;
    
    // 从标准输入读取一行数据
    std::getline(std::cin, line);
    
    // 将读取的数据输出到标准输出
    std::cout << "输入的行数据为：" << line << std::endl;
    
    return 0;
}
```

## 区别

1. **输入方式：** `cin`使用输入操作符（`>>`）从输入流中逐个读取数据，==以空格或换行符==作为分隔符。而`getline`函数则可以读取一整行数据，==包括其中的空格==，直到遇到换行符为止。
2. **数据类型：** `cin`对象可以直接将数据读取到相应的变量中，根据变量类型进行解析。例如，`cin >> num;`会将输入的数据解析为`num`的类型。而`getline`函数将读取的一行数据存储为字符串类型（`std::string`），无论输入的是数字还是其他字符。
3. **处理换行符：** `cin`对象的输入操作符（`>>`）会将输入流中的换行符留在输入流中。而`getline`函数在读取完一行数据后会自动忽略换行符，并将其丢弃，不会存储到字符串中。

# reverse函数

reverse（s.begin(),s.end(）)

左闭右开！

# C++ algorithm库中的几个常用函数

测试一下下
