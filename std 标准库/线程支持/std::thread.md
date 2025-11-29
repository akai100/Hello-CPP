# 1.创建线程

1. 使用**函数指针**创建线程

```c++
void print(const char* str)
{
    std::cout << str << std::endl;
}

std::thread th(print, "Hello World!");
```

2. 使用**lambda**创建线程

```c++
std::thread th([](const char* str){
    std::cout << str << std::endl;
}, "Hello World!");
```

3. 使用**类普通成员函数**创建线程

```c++
class MyTask {
public:
    void print(const char* str)
    {
        std::cout << str << std::endl;
    }
};

MyTask task;
std::thread th1(&MyTask::print, task, "Hello World!");
std::thread th2(&MyTask::print, &task, "Hello World!");
```

4. 使用**类静态成员函数**创建线程

```c++
class MyTask {
public:
    static void print(const char* str)
    {
        std::cout << str << std::endl;
    }
};
std::thread th(&MyTask::print_task, "Hello World!");
```

5. 使用**重载()类对象**创建线程

```c++
class MyTask {
public:
    void operator ()(const char* str)
    {
        std::cout << str << std::endl;
     }
};

MyTask task;
std::thread th(task, "Hello World!");
```

## 1.1 线程创建失败

```std::thread``` 构造时遇到错误（如系统线程上线、资源不足），会抛出```std::system_error```异常。

# 2. 线程管理

## 2.1 线程执行时间

在```std::thread```对象构造完成时立即开始执行。

## 2.2 线程同步

```std::thread::join()```实现线程同步，核心作用是阻塞调用线程、等待目标子线程完成，并回收线程资源。

+ 阻塞等待

  调用 join() 的线程（通常是主线程）会暂停执行，直到被调用的子线程执行完毕（线程函数返回、异常退出或执行完成）；

+ 资源回收

  子线程结束后，join() 会回收其内核资源（如 TCB 线程控制块、内核栈），避免 “僵尸线程”（占用内核资源但无实际作用）；

+ 状态切换

  子线程执行完毕后，对应的 std::thread 对象会从「joinable 状态」转为「non-joinable 状态」（可通过 joinable() 函数判断）；

```c++
void child_task() {
}

std::thread t(child_task);
t.join();
```

## 2.3 线程分离

线程分离值的是子线程转为后台线程，其生命周期不再受主线程控制，资源由操作系统自动回收。

核心功能：

+ 线程分离

  子线程与创建它的主线程解除关联，主线程不再需要（也无法）通过 join() 等待子线程结束；

+ 后台运行

  子线程转为 “后台线程”，其运行不依赖主线程 —— 主线程退出时，操作系统会强制终止所有未结束的后台线程；

+ 自动资源回收

  子线程执行完毕后，操作系统会自动回收其内核资源（TCB、内核栈），无需主线程调用 join() 回收；

+ 状态切换

  调用 detach() 后，std::thread 对象立即转为「non-joinable 状态」（joinable() 返回 false）；

void child_task() {
}


