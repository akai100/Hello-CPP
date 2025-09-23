# std::lock_guard

定义：
```C++
template<class Mutex>
class lock_guard;
```

作用：为在作用域期间占有互斥提供遍历 RAII 风格机制。

示例：

```C++

int g_i = 0;

std::mutex g_i_mutex;

void safe_increment()
{
    std::lock_guard<std::mutex> lock(g_i_mutex);
    ++g_i;
    // g_i_mutex 离开作用域时自动释放
}
```
