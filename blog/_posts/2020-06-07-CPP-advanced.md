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
|:--:|:--:|:--:|
|Unary traits| `is_integral<>` | if of type like bool, char, short, int, long |
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

