# std::thread

类```thread```表示单个执行ianc。

## 成员函数

### 构造函数

```C++
// 构造不表示线程的新 std::thread 对象
thread() noexcept;

// 移动构造函数
thread(thread && other) noexcept;

// 创建新的 std::thread 对象并将它与执行线程关联
template<class F, class... Args>
explicit thread(F && f, Args&&... args)

thread(const thread&) = delete;
```
