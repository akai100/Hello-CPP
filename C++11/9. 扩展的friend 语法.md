1、friend语法基础
C++中的友元机制允许类的非公有成员被一个类或者函数访问，友元按类型分为三种：普通非类成员函数作为友元，类的成员函数作为友元，类作为友元。友元包括友元的声明以及友元的定义。友元的声明默认为extern，也就是说友元类或者友元函数的作用域已经扩展到了包含该类定义的作用域。

（1）普通的非成员函数友元
```c++
class point {
public:
    point(double __x, double __y) : x_(__x), y_(__y) {}
    friend double distance(point &a, point &b);

private:
   double x_;
   double y_;
};

double distance(point &a, point &b)
{
    return sqrt((a.x_ - b.x_) * (a.x_ - b.x_) + (a.y_ - b.y_) * (a.y_ - b.y_));
}
```
（2）友元类
```c++
class B 
{ 
public: 
  B(void); 
  ~B(void); 
  int func(); 
}; 
class A 
{ 
friend class B;
public: 
  ~A(void); 
  A();
private:  
  int m_nItem;
}; 

int B::func() 
{ 
  cout<<"This is in B"<<endl; 
  A a;
  return a.m_nItem;
} 
```
（3）类成员函数作为友元函数
```c++
class A;
class B
{
public:
  B(void);
  ~B(void);
  int func(A xx);
};

class A
{
friend int B::func(A xx);
public:
  A(void):mx(20),my(30){}
  ~A(void){}
private:
  int mx;
  int my;
};

int B::func(A xx)
{
  return xx.mx * xx.my;
}
```
友元的特点：
+ 友元不具有相互性，只具有单项性
+ 友元不能被继承
+ 友元不具有传递性

2、C++11 对友元的改进
C++11 之前不支持对别名声明为友元，
