# std::async

函数模板```async```异步地运行函数```f```，并返回最终结果将保有该函数调用结果的 std::future。

```C++
// C++11起/C++17前
template<class Function, class... Args>
std::future<std::result_of_t<std::decay_t<Function>(std::decay_t<Args>...)>> async(Function&& f, Args&&... args);

// C++17起/C++20前
template< class Function, class... Args>
std::future<std::invoke_result_t<std::decay_t<Function>, std::decay_t<Args>...>> async( Function&& f, Args&&... args);

template< class Function, class... Args>
 [[nodiscard]]
std::future<std::invoke_result_t<std::decay_t<Function>, std::decay_t<Args>...>> async( Function&& f, Args&&... args );
```
