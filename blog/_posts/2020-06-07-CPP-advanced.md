---
title: Geometry
updated: 2020-04-17 17:00
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
for (auto& i : v)
for (const auto& i : v)
for (int j = 0; int i : {0, 1, 2})
```

## Smart pointers


## Variadic template