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

## Template metaprogramming

Metaprogramming is the writing of computer programs:
* That write or manipulate other programs (or themselves) **as their data**, or
* That do ... work at compile time that would otherwise be done at runtime.

C++ template metaprogramming uses template instantiation to drive compile time evaluation.
* When we use the name of a template where a {function, type, variable} is expected, the compiler will **instantiate** (create) the expected entity from that template. 
* Template metaprogrammer exploit this machinery to improve source code flexibility and runtime performance.

Metaprogramming does not deal with mutability (only const expr), virtual functions, RTTI (runtime type identification, e.g. auto), etc.

### Example: compile-time abs()

```
template<int N>     // template param used as metafunction param
struct abs {
    static_assert(N != INT_MIN);    // C++17-style guard
    static constexpr int value = (N<0) ? -N : N;    // "return"
};

int const n = ...;  // could instead declare as constexpr
... abs<n>::value ...;  //instantiation yields a compile-time constant 
```

### Example: compile-time recursion with specialization as base, greatest common divisor
```
template<unsigned M, unsigned N>
struct gcd {
    static int const value = gcd<N, M%N>::value;
};

// Partial specialization recognizes the base case gcd<m, 0>
template<unsigned M>
struct gcd<M, 0> {
    static_assert(M != 0);
    static int const value = M;
};
```

Metafunction can take types as parameters/arguments, e.g., `sizeof`. It can also produce a type as its result, e.g., type_traits.

### Example: obtain the (compile-time) rank of an array type
```
// primarily template handles scalar (non-array) types as base case:
template <class T>
struct rank { static size_t const value = 0u; };
// partial specialization recognizes any array type:
template <class U, size_t N>
struct rank<U[N]> { 
    static size_t const value = 1u + rank<U>::value; 
 };
```

### Example: "remove" a type's const-qualification
```
// primary template handles types that are not const-qualified
template <class T>
struct remove_const { using type = T; };
// partial specialization recognizes const-qualified types
template <class T>
struct remove_const<T const> { using type = T; };
```

### Example: IF (conditional in C++11)
```
template <class T>
struct type_is { using type = T; };
// primary template assumes the bool value is true
template<bool, class T, class>
struct IF : type_is<T> {};
// partial specialization recognizes a false value
template<class T, class F>
struct IF<false, T, F> : type_is<F> {};
```

`enable_if` is a single-type variation on conditional. If true, use the given type; if false, use no type at all.
```
// primary template assumes the bool value is true
template <bool, class T>     
struct enable_if : type_is<T> {};

template <class T>
struct enable_if<false, T> {};
```

### Example: SFINAE in use, want one algorithm f taking integral types T, and overload it with a second f taking floating-point types T
```
template <class T>
enable_if_t<is_integral<T>::value, int> f(T val) { ... };

template <class T>
enable_if_t<is_floating_point<T>::value, double> f(T val) { ... };
```

### Example: is_void
```
// primary template for non-void types
template <class T>
struct is_void : false_type {};
// specialization recognizes each void types
template <> struct is_void<void> : true_type {};
template <> struct is_void<void const> : true_type {};
```

### Example: is_same
```
// primary template for distinct types
template<class T, class U>
struct is_same : false_type {};
// partial specialization recognizes identical types
template<T>
struct is_same<T, T>: true_type {};
```

### Example: is_one_of
```
// primary template
template<class T, class... P0toN>
struct is_one_of;       // declare the interface only
// base #1: specialization recognizes empty list of types
template<class T>
struct is_one_of<T> : false_type {};
// base #2
template<class T, class... P1toN>
struct is_one_of<T, T, P1toN> : true_type {};
// specialization recognizes mismatch at head of list of types
template<class T, class P0, class... P1toN>
struct is_one_of<T, P0, P1toN> : is_one_of<T, P1toN> {};
```

### Example: testing for copy-assignability
```
template <class T>
struct is_copy_assignable {
private:
    template<class U, class = decltype(declval<U&>() = declval<U const&>())>
    static true_type try_assignment(U&&);   // SFINAE may apply!
    static false_type try_assignment(U&&);  // catch-all overload

public:
    using type = decltype(try_assignment(declval<T>()));
};
```

### void_t
```
template <class...>
using void_t = void;
```

### Example: detect the presence/absence of a type member named T::type
```
// primary template
template <class, class = void>      // default argument void is essential
struct has_type_member : false_type {};
// partial specialization if T::type exists
template <class T>
struc has_type_member<T, void_t<typename T::type>> : true_type {};
```

### Example: revisiting is_copy_assignable
```
template <class T>
using copy_assignable_t = 
    decltype(declval<T&>() = declval<T const&>());
// primary template
template <class T, class = void>
struct is_copy_assignable : false_type {};
template <class T>
struct is_copy_assignable<T, void_t<copy_assignable_t<T>>> : is_same<copy_assignable_t<T>, T&> {};
```

## Reference

[CppCon 2014: Walter E. Brown "Modern Template Metaprogramming: A Compendium, Part I"](https://www.youtube.com/watch?v=Am2is2QCvxY&ab_channel=CppCon)

[CppCon 2014: Walter E. Brown "Modern Template Metaprogramming: A Compendium, Part II"](https://www.youtube.com/watch?v=a0FliKwcwXE&ab_channel=CppConCppCon)