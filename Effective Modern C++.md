# Effective Modern C++

## 1.模板类型推导

C++中的模板类型推导（Template Type Deduction）是编译器根据函数模板调用时传入的实参类型，自动推导出模板参数类型的过程。

**基本概念**

在函数模板中，模板参数类型通常由编译器根据实参类型推导出来。例如：

```cpp
template<typename T>
void func(T param);

int main() {
    int x = 10;
    func(x); // 编译器将T推导为int
    return 0;
}
```

在上述代码中，`func`是一个函数模板，接受一个类型为`T`的参数。在调用`func(x)`时，编译器会根据实参`x`的类型（`int`）推导出模板参数`T`的类型为`int`。

**引用和指针的推导**

当函数模板的参数是引用或指针时，推导规则会有所不同。例如：

```cpp
template<typename T>
void func(T& param);

int main() {
    int x = 10;
    const int cx = 20;
    func(x);   // T推导为int
    func(cx);  // T推导为const int
    return 0;
}
```

在这个例子中，`func`接受一个`T&`类型的参数。当传入`x`时，`T`被推导为`int`；当传入`cx`时，`T`被推导为`const int`。需要注意的是，`const`修饰符会被保留在`T`中。

**万能引用（Universal Reference）**

C++11引入了万能引用的概念，即在模板函数中，参数类型为`T&&`时，`T`的推导规则会根据传入实参的值类别（左值或右值）进行调整。例如：

```cpp
template<typename T>
void func(T&& param);

int main() {
    int x = 10;
    func(x);    // T推导为int&
    func(10);   // T推导为int
    return 0;
}
```

在这个例子中，`func`接受一个`T&&`类型的参数。当传入左值`x`时，`T`被推导为`int&`；当传入右值`10`时，`T`被推导为`int`。这种行为是由于引用折叠规则（Reference Collapsing）导致的。

**引用折叠规则**

引用折叠规则决定了在模板参数推导过程中，引用类型如何合并。具体规则如下：

- `& &` → `&`
- `& &&` → `&`
- `&& &` → `&`
- `&& &&` → `&&`

这些规则解释了为什么在某些情况下，传入左值时，`T`会被推导为引用类型，而传入右值时，`T`会被推导为值类型。

**类模板的类型推导**

从C++17开始，类模板也支持类型推导。例如：

```cpp
template<typename T>
class MyClass {
public:
    MyClass(T val) : value(val) {}
private:
    T value;
};

int main() {
    MyClass obj(10); // T推导为int
    return 0;
}
```

在这个例子中，`MyClass`是一个类模板，接受一个类型为`T`的构造函数参数。在创建`obj`时，编译器会根据传入的实参`10`推导出`T`的类型为`int`。

**总结**

C++的模板类型推导机制使得代码更加简洁和灵活。理解推导规则对于编写高效且易于维护的代码至关重要。在实际编程中，合理利用模板类型推导，可以减少冗余代码，提高代码的可读性和可维护性。
