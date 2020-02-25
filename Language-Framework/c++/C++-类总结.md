# C++ class 总结



## 访问属性

* **public :**  类内 可访问， 类的用户可访问
* **protected :** 类内可访问， 类的用户不可访问
  * 派生类类内可访问
  * 派生类的 友元 可以访问 派生类的 protected 属性，不可访问 基类的 protected 属性。 
* **private :** 类内 可访问，类的 用户不可访问。
  * 派生类 类内不可访问， 派生类的用户不可访问。



## 基本语法





## 常量对象？？？



## 继承

**语法：**

```c++
class Y : public X{
  
};// 这是一个声明语句，所以后面要加 分号， 语句块的话就不需要加分号。
```

**Y继承 X：**

* Y 将 包含 X 中所有 数据成员和成员函数。正如没有继承 X， 直接在 Y 中创建一个 X 的成员一样，Y 是包含了一个 X 的子对象。
* 继承 依旧 遵守保护机制。即：`X` 中的私有成员 在 `Y` 中不能直接访问。



**几种继承**

* **public 继承** : 继承下来的 数据成员和 成员函数 保持之前的访问属性。
* **protected 继承**：继承下来的 public 变成了 protected
* **private 继承：** 继承下来的 数据成员 和 成员函数 的访问属性都变成 私有，即 `Y` 内可访问，`Y` 外不可直接访问。



**重写：override**

* 重新 定义 基类继承下来的 成员。



## 多态

**多态： 即， 晚捆绑， 通过 虚函数 实现**

**为了实现 晚捆绑，C++ 要求在基类 `声明这个函数时` 使用 virtual 关键字**

* 晚捆绑仅对  `virtual` 函数起作用
* 只在使用 含有 `virtual` 函数的基类的地址时发生。
* 仅仅需要在声明是 使用 `virtual`， 定义时并不需要。仅仅需要在 基类中声明 `virtual` 函数。 
* 派生类中 `virtual` 函数的重定义 常常称为 重写 (`override`)



**C++如何实现 晚捆绑**

* 对每个 包含虚函数的 类创建一个 表 (`VTABLE`)
* 在 `VTABLE` 中，编译器放置 特定类的 虚函数地址。
* 在每个 带有虚函数的 类中， 编译器秘密地放置一个指针，称为 `vpointer`， 指向这个对象的 `VTABLE`
* 当通过 **基类指针做虚函数调用时** ，编译器静态插入地 插入 **能取得 这个 `VPTR` 并在 `VTABLE` 表中查找函数地址的** 代码. 这样就能正确的调用函数 并引起晚捆绑的发生。





```c++
#include <iostream>

using namespace std;

class X {
public:
    virtual void print() {
        cout << "i am in X" << endl;
    }

    void call_print() {
        // 等价于 this->print();
        print();
    }
};

class Y : public X {
public:
    void print() override {
        cout << "i am in Y" << endl;
    }
};

int main() {
    X *x = new Y;
    x->call_print();

    std::cout << "Hello, World!" << std::endl;
    return 0;
} // 输出： i am in Y
```



## 删除编译器提供的默认 方法

以下方法, 如果用户没有显式实现, 编译器则会提供一个对应的默认方法

* 构造函数: `A(){}`
* 拷贝构造函数`A(const A &a)`
* 赋值函数 `A& operator=(const A &rhs)`
* 析构函数 `~A()`

如何我们并不想编译器默认生成这些方法的话, 可以使用 `=delete` 关键字

* `A()=delete;`



## 如何避免隐式类型转换

C++ 会有一些隐式类型转换的场景, 比如:

* 赋值的时候
* 传参的时候

如果不想用隐式类型转换的话, 可以给构造函数加一个 `explicit` 关键字



