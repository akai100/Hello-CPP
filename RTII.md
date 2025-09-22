运行时类型识别（RTTI，Run-Time Type Information）是一种在程序运行时获取和操作对象类型信息的机制，主要用于支持多态场景下的类型安全操作。

## dynamic_cast

作用：安全地将基类指针或引用转换为派生类型指针或引用（向下转型）。若转换失败：

+ 指针类型返回```nullptr```
+ 引用类型抛出```std::bad_cast```异常

限制：仅适用于含虚函数的类（多态类型），依赖虚函数表（vtable）存储类型信息。

## typeid

作用：返回```const std::type_info&```对象，包含类型名称等信息。

## type_info

作用：存储类型元数据（如类型名称字符串），构造函数为```protected```，由编译器生成实例。

实现：其地址保存在虚函数表的首槽位（slot 0）

## 实现原理

虚函数表（vtable）关联：RTTI 信息通过 vatble 绑定，多态类的每个对象隐含一个 vptr 指针，指向包含 type_info 地址的虚表。

编译器与ABI依赖：

启用条件：需编译器开启 RTTI 支持（GCC/Clang 默认启用 -frtti，禁用-fno-rtti）

## 应用场景

使用场景：
+ 安全向下转型
+ 调试与日志
+ 异常处理

