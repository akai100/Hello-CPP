## 1. 类与默认函数
默认函数：在C++中自定义的类，编译器会默认帮助程序员生成一些他们未定义的成员函数，包含：
+ 构造函数
+ 拷贝构造函数
+ 拷贝赋值函数(operator=)
+ 移动构造函数
+ 移动拷贝函数
+ 析构函数

除此外，还会提供全局默认操作符函数：
+ operator,
+ operator &
+ operator &&
+ operator *
+ operator ->*
+ operator new
+ operator delete

在C++语言规则中，一旦实现了这些函数的自定义版本，则编译器不会再为该类自动生成默认版本。可能出现的问题：一旦申明了自定义版本的构造函数，则有可能导致我们定义的类型不再是POD的。例如：
```c++
class TwoCstor {
public:
    TwoCstor() {};    // 提供了带 参数版本的构造够函数，需自行提供，且TwoCsTor不再是POD类型

    TwoCstor(int i) : data(i) {}

private:
   int data;
};
```

在C++11中，标准通过提供了新的机制来控制默认版本函数的生成来”恢复“POD的特质。这个新机制重用了default关键字，在代码中可以在默认函数定义或者声明是加上”= default“，从而显示地指示编译器生成该函数的默认版本。例如：
```c++
class TwoCstorV2 {
public:
    TwoCstorV2() = default;

    TwoCstorV2(int i) : data(i) {}

private:
   int data;
};
```
## 2. delete：限制默认函数的生成
在函数的定义或者声明加上“= delete”，指示编译器不生成函数的缺省版本。例如:
```c++
class NoCopyCstor {
public:
    NoCopyCstor() = default;

    NoCopyCstor(const NoCopyCstor &) = delete;
};
```

无法调用NoCopyCstor的拷贝构造函数。

## 3. ”= default" 和 "= deleted"
在C++11 标准中，"= default"修饰的函数成为显示缺省函数，"= delete"修饰的函数称为"删除函数"。
C++11 引入显示缺省和显示删除是为了增强对类默认函数的控制，让程序员能够更加精细地控制默认版本的函数。
### 3.1 显示缺省
显示缺省不仅可以再类的定义中修饰成员函数，也可以在类的定义之外修饰成员函数。例如：
```c++
// 使用default 在类定义之外修饰成员函数
class DefaultedOptr {
public:
    DefaultedOptr() = default;

    DefaultedOptr & operator = (const DefaultedOptr &);
};

inline DefaultedOptr & DefaultedOptr::operator=(const DefaultedOptr &) = default;
```
在类定义外显示指定缺省版本的好处是可以对一个类定义提供多个实现版本。
### 3.2 显示删除
显示删除可以避免用户使用一些不应该使用的类的成员函数，但是并不局限于成员函数，使用显示删除还可以避免编译器做一些不必要的隐式数据类型转换：
```c++
class ConvType {
public:
    ConvType(int i) {};
    
    ConvType(char c) = delete;  // ConvType cc('a');  无法通过编译
};
```
