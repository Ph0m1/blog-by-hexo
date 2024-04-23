---
title: "Note about C++"
description: "C++ 笔记"
tags: ["c++"]
date: 2022-03-11T16:01:23+08:00
draft: true
---
## 友元

友元可以访问类的私有成员。

```cpp
class MyClass {
    friend void myFriendFunction(MyClass&);
    private:
        int myPrivateInt;
};

void myFriendFunction(MyClass& myClass) {
    std::cout << myClass.myPrivateInt << std::endl;
}

int main() {
    MyClass myObject;
    myObject.myPrivateInt = 42;
    myFriendFunction(myObject);
    return 0;
}
```

```cpp
class Tom;
tamplate <typename T>
class MyClass {
public:
    frind T;
private:
    int age;
    int height;
    int weight;
};


```

## 继承

### 访问控制

- 基类的 public 成员在派生类中保持 public
- 基类的 protected 成员在派生类中保持 protected
- 基类的 private 成员在派生类中变为 private

### 继承方式

- 单继承
- 多继承
- 虚继承

### 虚函数

- 基类中的虚函数在派生类中仍然为虚函数
- 派生类中的虚函数会覆盖基类中的虚函数
- 派生类中没有声明为虚函数的函数不会被覆盖

### 虚基类

- 虚基类可以减少派生类中重复的基类成员

### 纯虚函数

- 纯虚函数在基类中声明，没有定义
- 纯虚函数的声明方式为 `virtual 返回类型 函数名(参数列表) = 0`
- 纯虚函数的作用是让基类成为抽象类
- 抽象类不能被实例化
- 抽象类中可以包含非纯虚函数

### 虚析构函数

- 虚析构函数在基类中声明，没有定义
- 虚析构函数的作用是让基类中的指针指向派生类对象时，能够正确释放派生类对象占用的内存

### 虚继承

- 虚继承可以解决菱形继承问题
- 虚继承的基类中包含虚析构函数
- 虚继承的基类中包含虚函数

## 模板

### 函数模板

- 函数模板可以接受任意类型的参数和返回值    

### 类模板


- 类模板可以接受任意类型的参数和返回值


### 模板特化

- 模板特化可以针对特定的类型进行优化


## 异常

### 异常处理

- 异常处理可以捕获异常并处理异常

### 异常抛出


### 异常捕获

- 异常捕获可以捕获异常