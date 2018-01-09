---
title: C++ Programming
updated: 2017-03-22 13:50
category: [Programming]
tags: [C++]
---

* TOC
{:toc}

## 强制类型转换

比起C风格的强制类型转换(type-id)，C++的标准类型转换操作更为安全。包括：

1. static_cast<>(). (1)把子类指针或引用转为基类，但反向则不安全；（2）基本类型转换，如把int转为enum.
2. dynamic_cast<>(). 常用于进行类的上下行转换，但下行转换(基类转为子类)比static安全，因为它会对类型进行检查。所以，**涉及到有继承关系(包括父子、兄弟)的类的转换一律用dynamic_cast**. 若返回空指针则表示转换失败。
3. const_cast<>(). 作用于const对象。将常量指针转换为非常量指针，常量引用转换为非常量引用，且仍然指向或引用同一对象。
4. reinterprete_cast<>(). 多用于函数指针类型的转换，不常用。


## Virtual Contructor
A virtual constructor is the ability to clone an object, without actually knowing what type it is. This is very useful when we do not know the real type of an object, but need a copy of it. Let's say we have a base-class thusly:

```
class Object
{
public:
    Object() {};
    virtual ~Object(){} = 0;
    virtual Object* clone() const = 0;
};
```

And derived class:
```
class MyClass : public Object
{
public:
    MyClass() {};
    MyClass(const MyClass& rhs) {}; // copy-constructor
    virtual ~MyClass() { };
    virtual MyClass* clone() const { return new MyClass(*this); }; // virtual ctor
};
```
Now, suppose we have a vector of these Object-derived classes and we want to make a copy of this vector:
```
std::vector<Object*> objects;
objects.push_back(new MyClass());
objects.push_back(new AnotherClass());

std::vector<Object*> anotherVector;
for(std::vector<Object*>::const_iterator cit(objects.begin());
    cit != objects.end();
    ++cit)
    anotherVector.push_back((*cit)->clone());
```

## 智能指针
在C++程序中，异常的产生经常会导致内存的泄漏，因为异常的发生导致delete语句无法被执行。为了实现更安全的内存管理，应使用智能指针指向内存。智能指针也是一个对象，指向的内存是该对象所拥有的member。因此，智能指针相当于在内存外包装了一层安全的外衣，即使有异常产生，智能指针的析构函数依然能完成内存的释放。C++11中引入了重要的智能指针：shared_ptr。使用时需包含头文件`#include<memory>`。

**Shared_ptr**是最智能的智能指针。它通过引用计数的方式来管理指针资源。同一个指针资源可以被多个shared_ptr对象所拥有。当最后一个shared_ptr对象的析构函数被执行时才会最终释放所拥有的内存。

```
shared_ptr<int> spInt1 = make_shared<int>(10);
//make_shared函数用于创建shared_ptr对象
//或者也可使用shared_ptr<int> spInt1(new int(10));
shared_ptr<int> spInt2 = spInt1;
shared_ptr<int> spInt3(spInt1);
//赋值和拷贝会增加指针资源的引用计数，而不会拷贝内存
spInt1.use_count()	//获取拥有该指针资源的引用计数
spInt1.reset()	//取消该指针对指针资源的所有权，该指针变为空指针。
//如果没有其他指针拥有该指针资源，则会调用析构函数释放内存；
//如果仍有其他指针，则其他指针的引用计数会减一，内存暂不释放。
```

对shared_ptr进行类型转换不能使用一般的`static_cast<T>()`，`dynamic_cast<T>()`，`const_cast<T>()`，而应使用量身定制的类型转换操作符`static_pointer_cast<T>()`，`dynamic_pointer_cast<T>()`，`const_pointer_cast<T>()`。

```
const shared_ptr<T> p;		//p is const
shared_ptr<const T> p;		//*p is const
```
## Implicit类型转换
```
class Rational
{
public:
Rational(int, int);
operator double() const;	//convert Rational to double, note no return value for this function
}
```
以上是一种自定义类型转换的方式，将double转化为自定义的类，以后当碰到运算如`0.5*Rational(1, 2)`时，Rational类的object会被显式的转换为double类型。
```
class Array
{
public:
Array(int size);
}
```
另一种隐式类型转换发生在constructor只包含一个输入参数（或多个输入参数，但除第一个参数外其他参数都有默认值）。这种转换通常是比较危险的，为了规避此种情况，通常在constructor的声明前加上**explicit**关键字以进制隐式转换：`explicit Array(int size);`。

## Re-understand `new` and `delete`
```
void * memory = operator new(sizeof(string));
// operator new: allocate raw memory

new Widget(size);
// new operator: automatically allocate memory and construct object in it

#include <new>
void * buffer = mallocShared(sizeof(Widget));
Widget* pw = new (buffer) Widget(size)	;
// placement new, construct object within specified memory pointed by `buffer`


operator delete(memory);
// operator delete: deallocate raw memory

delete Widget;
// delete operator: deconstruct object then deallocate memory

pw->~Widget();		
// desconstruct object first
freeShared(pw);		
// deallocate memory as paired with mallocshared

string *ps = new string[10];	
// call operator new [] to allocate memory and call constructor for each element

delete [] ps;
// call destructor for each element and call operator delete [] to deallocate memory
```

## How to materialize AtomicWrite
When we are talking about atomic write, we desire that eigher all of the writes have to make it to the disk or none of them.
The general routine is to first write a temporary file (e.g. in `AtomicWriteHelper`'s constructor) and then rename it the final destination (e.g. in `AtomicWriteHelper`'s destructor).

## Structures in C and C++
`struct` and `class` are almost the same thing in C++. The only difference is that the default visibility of `struct` is public instead of private in `class`. `struct` in C does not support the `private`, `protected` and `public` specifiers.

## Exception
```
#include <exception>
using namespce std;

class myexception: public exception
{
	virtual const char* what() const throw()
	{
		return "My exception happened";
	}
} myex;

int main () {
try {
	try {
		throw myex;
	}
	catch (exception & e) {
		throw;			// forward exception to external
	}
catch (exception & e) {
	cout << e.what() << '\n';
}
return 0;
}
```

## Stack VS. Heap

|      | Stack  | Heap |
|:----:|:------:|:-----:|
|Store | RAM    | RAM   |
|Scope |Variables created on the stack will go out of scope and are automatically deallocated.|Variables created on the heap by `new` or `malloc` must be destroyed manually by `delete` or `free` and never fall out of scope.
|Usage | Local data| Global data (especially shared by multi-thread) |
|Ownership | Stack is allocated for each system-level thread. | Heap is allocated for application.|
|Speed | Fast (LIFO for stack) | Slow (More complex for heap to track allocation)|
|Risk | Stack overflow if too much stack is used (e.g. too deep recursion, too large allocation) | Memory leak|

## Keyword: `mutable`
An example tells everything:
```
class HashTable {
 public:
    //...
    std::string lookup(const std::string& key) const
    {
        if (key == last_key_) {
            return last_value_;
        }

        std::string value{this->lookupInternal(key)};

        last_key_   = key;
        last_value_ = value;

        return value;
    }

 private:
    mutable std::string last_key_
    mutable std::string last_value_;
};
```
在实现Hash表时，一个提升访问效率的方法是保存最近一次访问的item，这样再次访问相同item时直接返回上一次获取的结果即可，即`last_key_`，`last_value_`，节省了重复查表的开销。但是我们又希望查表函数`lookup`是`const`，以保护其他成员变量。为此一个折中的方法是将`last_key_`，`last_value_`声明为`mutable`，使得即使是`const`函数都可以对之做修改。

## Keyword: `auto`
在C++11中，关键字`auto`用于根据上下文对变量类型进行推导。其用法注意点如下：
```
// auto声明的变量必须初始化
auto m;	// not allowed
```
```
// 可以用pointer，reference，const来修饰auto
auto k = 5;
auto* pk = new auto(k);
autp** ppk = new auto(&k);
const auto n = 10;
```
```
// 函数和模板参数不能被声明为auto
void Func(auto param);
template<auto T>
void Func(T t);
```
```
// auto无法被自动推导成const或volatile
const int a = 100;
auto b = a;	// typeof(b) = int, changeable
auto & c = a;	// typeof(c) = const int, unchangeable
```
```
// auto与数组
int a[10];
auto b = a;	// typeof(b) = int*
auto & c = a;	// typeof(c) = int[10]
```

## Keyword: `volatile`
首先来看一个例子：
```
int some_int = 100;
while(some_int == 100)
{
   //your code
}
```
上面的代码段是一个`while`死循环，循环内部没有对变量`some_int`进行任何修改。一般上，编译器在编译上述代码时会做一步优化：将`while(some_int == 100)`解释为`while(true)`。这种变通在正常情况下显然是合理且可取的，因为它避免了每次循环都去访问`some_int`的值，降低了内存访问的开销。但是，如果在`while`循环的同时有其他程序会修改`some_int`的值，这种变通就会造成错误。解决这个问题的方法就是使用`volatile`关键字：`volatile int some_int = 100;`，避免编译器的过度优化，迫使程序每次获取改变量时都必须访问其地址。

引用官方文档的解释：*...volatile is a hint to the implementation to avoid aggressive optimization involving the object because the value of the object might be changed by means undetectable by an implementation.*

## Member function template cannot be virtual

In other words, virtual member function cannot use template.
> Member function templates cannot be declared virtual. This constraint is imposed because the usual implementation of the virtual function call mechanism uses a fixed-size table with one entry per virtual function. However, the number of instantiations of a member function template is not fixed until the entire program has been translated. Hence, supporting virtual member function templates would require support for a whole new kind of mechanism in C++ compilers and linkers. In contrast, the ordinary members of class templates can be virtual because their number is fixed when a class is instantiated.

翻译过来大意是：virtual function的调用都是通过virtual table来实现的，table中的每一个entry对应某个类中的virtual function。如果是模板的话，一个entry则对应了类中的无数个virtual function。

## static

#### Function statics VS. Class statics
>An object that's static in a class is, for all intents and purposes, always constructed (and destructed), even if it's never used. In contrast, an object that's static in a function is created the first time through the function, so if the function is never called, the object is never created.
>
>Compilers typically use a hidden flag variable to indicate if the local statics have already been initialized, and this flag is checked on every entry to the function.

#### Create a single copy of object by static

The goal here is to create zero or a unique object of certain class (e.g. a single `Printer` for sharing). More than one creation is prohibited.

```
class Printer {
public:
	void print(PrintJob const & job);
	...
	friend Printer & uniquePrinter();
private:
// the constructor is private so that it cannot be created normally
	Printer();
	Printer(Printer const & rhs);
};

// friend function can access private constructor
// function static ensures uniqueness no matter how many times the function is called
Printer & uniquePrinter()
{
	static Printer p;
	return p;
}

// usage
uniquePrinter().print(job);
```

#### 继承性
在基类中声明的static变量是由基类和子类共享的。



## Reference
[http://www.cnblogs.com/chio/archive/2007/07/18/822389.html](http://www.cnblogs.com/chio/archive/2007/07/18/822389.html)

[More Effective C++: 35 New Ways to Improve Your Programs and Designs](http://www.physics.rutgers.edu/~wksiu/C++/MoreEC++_only.pdf)

[https://stackoverflow.com/questions/10157122/object-creation-on-the-stack-heap](https://stackoverflow.com/questions/10157122/object-creation-on-the-stack-heap)

