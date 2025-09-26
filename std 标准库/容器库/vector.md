# vector

vector 元素被连续存储。

定义：

```C++
template<class T, class Allocator = std::allocator<T>>
class vector;
```

操作复杂度：

+ 随机访问 —— O(1)
+ 在末尾插入或移除元素 —— O(1)
+ 插入或移除元素 —— O(n)
