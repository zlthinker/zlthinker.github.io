---
title: C++ Advanced
updated: 2020-06-07 11:00
category: [CV]
tags: [Notes]
---

* TOC
{:toc}

## Lambda expression

```
[]() mutable ->return type
{

}
```

* The capture clause introduce the variables from the surrounding scope by value ```[var]``` or by reference ```[&var]```. ```[=]``` capture all the outside variables by value and ```[&]``` capture all the outside variables by reference.
* If you capture a outside variable by value and you want to change its value, you need to add the ```mutable``` keyword.s
* ```->return type``` is optional as the return type can be automatically deduced.


## Container iterators

#### Transform

```
// Manipulating one container with unary operator
std::Transform(A.begin(), A.end(), B.begin(), unary_function);  // output to B

// Manipulating two containers with binary operator
std::Transform(A.begin(), A,end(), B.begin(), C.begin(), binary_function); // output to C
```

#### for_each

```for_each``` can only manipulate one container.

```
std::for_each(A.begin(), A.end(), unary_function)   // return the the unary function
```

#### for (auto a : A)

```
for (auto i : v)
for (auto&& i : v)  // when you want to modify the elements
for (const auto& i : v)
for (int j = 0; int i : {0, 1, 2})
for (auto [first, second] : map)
```

## Smart pointers


## Variadic templates

```
template<typename... Args>
void Foo(Args... args) {
}

// You can call it by
Foo(1.0);
Foo("string", bool, 10);
```

Variadic templates can be used to implement recursive functions. For example,

```
template<typename T>
T Sum(T arg) {
    return arg;
}

template<typename... Args>
T Sum(T t, Args... args) {
    return t + Sum(args...);
}
```

## type_traits

Type traits are used to in template programming to support type inference and type transformation **at compile time**. There are three types of `type_traits`: 

1. Unary type traits: describe a property of a type;

2. Binary type traits: describe a relationship between two types;

3. Transformation traits: modify a property of a type.

| Traits types | Examples | Function |
|:--:|:--|:--|
|Unary traits| `is_integral<>` | if of types like bool, char, short, int, long |
|| `is_void<>` | if of type void |
|| `is_floating_point<>` | if of type floating point |
|| `is_const<>` | if of constant type |
|| `is_pointer<>` | if of pointer type |
|| `is_array<>` | if of array type |
|| `is_class<>` | if it is a class |
|| `is_enum<>` | if it is a enum |
|| `is_function<>` | if it is a function type |
|Binary traits| `is_same<T,U>`| if T and U are the same type|
|| `is_base_of<T,U>`| if T is the base type of U|
|| `is_convertible<T,U>`| if T can be converted to U|
|Transformation traits| `remove_const<>`| const int -> int|
|| `add_const<>`| int -> const int|
|| `add_pointer<>`| int -> int*|
|| `remove_pointer<>`| int* -> int|
|| `remove_all_extent<>`| int[][][] -> int|
|| `enable_if<>`| used in **SFINAE**|


## std::thread

```
std::thread t(func, args);
t.join();       // It blocks the main thread, waiting for thread t to finish
t.detach();     // Thread t is indepdent from the main thread. All resources will be released automatically after t finishes.

std::this_thread::sleep_for(std::chrono::seconds(5));
std::this_thread::get_id();
```

### Mutex (Mutual Exclusive)

```
std::mutex m;
m.lock();       // If thread 1 acquires this lock, thread 2 that wants to acquire this lock will be blocked.
...
m.unlock();

m.try_lock();   // If thread 1 acquires this lock, thread 2 will try to acquire it (fail, return false, no block).
...
m.unlock();

std::recursive_mutex m;
m.lock();
    {
        m.lock();       // call lock() recursively
        ...
        m.unlock();     // unlock and lock should appear in pairs
    }
m.unlock();

m.lock();
//exception happens, unlock() will never be called.
m.unlock();

std::mutex m;
std::lock_guard<std::mutex>(m);     // call lock() in constructor, unlock() in destructor

std::shared_mutex m;
std::shared_lock<std::shared_mutex> sl(m);  // simultaneous read access of multiple threads
std::unique_lock<std::shared_mutex> ul(m);  // exclusive write access
```

### condition_variable
```
std::condition_variable cv;
std::mutex m;
// thread 1
{
    std::unique_lock<std::shared_mutex> ul(m);
    cv.wait(ul, condition);     // condition = false, lock not acquired, thread waiting
                                // condition = true, lock acquired, thread running
}
// thread 2
{
    std::unique_lock<std::shared_mutex> ul(m);
    condition = true;
    cv.notify_one();            // notify waiting thread
}
```

### Dead lock
```
std::mutex m1, m2;
// thread 1
{
    std::lock_guard<std::mutex>(m1);    // thread 1 acquires m1
    std::lock_guard<std::mutex>(m2);    // thread 1 waits for m2
}
// thread 2
{
    std::lock_guard<std::mutex>(m2);    // thread 2 acquires m2
    std::lock_guard<std::mutex>(m1);    // thread 2 waits for m1
}
```

### Atomic (lock free)
```
#include <atomic>   
// for integral types
atomic_bool = std::atomic<bool>
atomic_char = std::atomic<char>
atomic_int = std::atomic<int>
atomic_long = std::atomic<long>
atomic_size_t = std::atomic<size_t>
```

## SFINAE

### enable_if
```
template <bool, typename T = void>
struct enable_if
{};

template <typename T>
struct enable_if<true, T> {
  typedef T type;               // type is only defined for bool = true
};

typename enable_if<true, int>::type t;  // define an int variable t
```

```
template <typename T>
typename std::enable_if<std::is_integral<T>::value>::type foo(T value)
{
    // an implementation for integral types (int, char, unsigned, etc.)
}

template <typename T>
typename std::enable_if<!std::is_integral<T>::value>::type foo(T value)
{
    // an implementation for non integral types
}

foo(100);   // call the first foo() since 100 is an integer
foo("abc")  // call the second foo() since "abc" is not integral
```

### use enable_if in template
```
template <class T,
         typename std::enable_if_t<std::is_integral<T>::value>* = nullptr>
void foo(T& t) {
    // an implementation for integral types (int, char, unsigned, etc.)
}

template <class T,
          typename std::enable_if_t<!std::is_integral<T>::value>* = nullptr>
void foo(T& t) {
    // an implementation for non integral types
}

foo<int>(100);              // call the first foo() since 100 is an integer
foo<std::string>("abc")     // call the second foo() since "abc" is not integral
```
