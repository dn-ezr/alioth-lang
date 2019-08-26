---
title: "Appendix B: Features in comming"
author: GodGnidoc
date: 2019/08/25
---

# Appendix B: Features in comming

This page is used to describe features in comming. They are only thoughts in my mind, so it's not very sure that they will be introduced in future language version.

- [Appendix B: Features in comming](#appendix-b-features-in-comming)
  - [Combined concepts](#combined-concepts)
    - [Quasi method](#quasi-method)
  - [Multiple reture types](#multiple-reture-types)
  - [Operator in transparent class](#operator-in-transparent-class)
  - [Multiple return value](#multiple-return-value)

## Combined concepts

### Quasi method

To avoid concept conflict, I desied to combine the concept of `virtual method` into the concept of `quasi method`.

## Multiple reture types

We can declare more than one return types when defining method, this also makes the method a `quasi method`, difference is that this method can only return data object of which is the type of those types declared.

~~~
class Hello {
    method sayHello( ) int32, Error
}
~~~

## Operator in transparent class

We can define operators in the `transparent class`. That makes those operators `meta operator`, which means you have to declare two parameters for binary operators, and on parameter for mono operators.

~~~
class Math {
    operator + ( v1 vector, v2 vector ) vector
}
~~~

emmmmmm, this is a thought quite useless.

## Multiple return value