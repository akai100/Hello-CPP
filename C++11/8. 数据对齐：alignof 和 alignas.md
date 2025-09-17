## 1. 什么是数据对齐？

通常情况下，C/C++结构体中的数据会有一定的对齐要求，

（2）为什么要数据对齐：
+ 读写性能上的优势。
+ 在某些平台。硬件将无法读取不按字节对齐的某些类型数据，硬件会抛出异常来终止程序

（3）关于数据对齐的一个例子：
```c++
struct HowManyBytes {
   char a;
   int b;
};

cout << "sizeof(char): " << sizeof(chat) << endl;
cout << "sizeof(int): " << sizeof(int) << endl;
cout << "sieof(HowManyBytes): " << sizeof(HowManyBytes) << endl;

cout << "offset of char a: " << offsetof(HowManyBytes, a) << endl;
cout << "offset of int a: " << offsetof(HowManyBytes, b) << endl;
```
运行上面的代码我们可以得到下面的输出：

![e955a09fc35123e096d15b53f3daa109.png](:/87925bcf71d647e195a433d414f019c3)

HowManyBytes如果只考虑char类型成员a 和 int类型成员b的大小，其大小sizeof应该是5，但是输出是8，这是因为数据对齐导致的。通过offsetof查看成员的偏移位置，成员的a偏移位置为0，但是成员b的偏移位置为4，这是因为在平台定义上，C/C++的int类型数据要求对齐到4字节，即要求int类型数据必须放在一个能够整除4的地址上，因为前面成员a占了一个字节，b对齐到第二个4字节上，造成3字节的空间被空出，通常称因为对齐而造成的内存留空未填充数据。

每个类型的数据除去长度等属性之外，还有一项”被隐藏属性“，那就是对齐方式，对于每个内置或者自定义类型，都存在一个特定的对齐方式。对齐方式通常是一个整数，它表示的是一个类型的对象存放的内存地址应满足的条件，称为对齐值。

## 2. C++11 alignof 和 alignas
C++11在新标准中为了支持对齐，引入两个关键字：操作符alignof、对齐描述符(alignment-specifier)alignas。
+ alignof
表示一个定义完整的自定义类型或者内置类型或者变量，返回的值是一个std::size_t类型的整型常量。如同sizeof操作符一样，alignof获得的也是一个与平台相关的值。
+ alignas
alignas既可以接收常量表达式，也可以接收类型作为参数，如:
```c++
alignas(double) char c;
```
使用效果和下面一样：
```c++
alignas(alignof(double)) char c;
```
