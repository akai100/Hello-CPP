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

## 构造函数

(1) 默认构造函数

```C++
vector() : vector(Allocator()) {}    // C++11 起，C++17 前

vector() noexcep(noexcept(Allocator())) : vector(Allocator()) {}    
```

（2）构造拥有给定分配```alloc```的空```vector```

```C++
// C++11 前
explicit vector(const Allocator& alloc = Allocator());

// C++11 起，C++17 起为 noexcept，C++20 起为 constexpr
explicit vector(const Allocator& alloc)
```

（3）构造拥有```count```个默认插入的 T 对象的 ```vector```。不进行复制

```C++
// C++11 起
explicit vector( size_type count, const Allocator& alloc = Allocator() );
```

（4）构造拥有 count 个值 value 的元素的 vector

```C++
// C++11 前
explicit vector( size_type count, const T& value = T(), const Allocator& alloc = Allocator() );

// C++11 起
vector( size_type count, const T& value, const Allocator& alloc = Allocator() );
```
