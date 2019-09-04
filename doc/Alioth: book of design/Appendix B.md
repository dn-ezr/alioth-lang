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
  - [Calling stack expression](#calling-stack-expression)
  - [Method entity structure](#method-entity-structure)
  - [Dependency set in module signature](#dependency-set-in-module-signature)
  - [Overriding methods by changing return prototype](#overriding-methods-by-changing-return-prototype)
  - [Leave away any scope immediatly](#leave-away-any-scope-immediatly)
  - [Break or continue one block after doing something](#break-or-continue-one-block-after-doing-something)

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

Make method return more than one return value. This thought came from a professor we met when we were attending the "4C".

## Calling stack expression

I'm thinking that we can express the entry in the calling stack while executing method to decide what to do according to the calling information.

## Method entity structure

Generate static structures to store attributes for methods, these attributes can be located by combined keyword `this method`.

## Dependency set in module signature

Use brackets to enclose modules from one package to reduce code count.

Example :

~~~
module HelloWWW :
  { io net string memory } @ stdlib
~~~

The code shown above makes the equal effect as the following code:

~~~
module HelloWWW :
  io @ stdio
  net @ stdio
  string @ stdio
  memory @ stdio
~~~

This maybe make it easier to code, but the complexity of the syntax analyser integrated in the lexical analyser will increased.

## Overriding methods by changing return prototype

Make it possible to change the return prototype only, when overriding methods.

Example:

~~~
method getDocument( ref const filename string ) ref Document
method getDocument( ref const filename string ) string
~~~

Use the modifier `?` to choose which the method you are calling.

Example:

~~~
getDocument?string( "source.alioth" );
~~~

## Leave away any scope immediatly

Use the combined keyword `break!` to change the semantics of the statement `break`.

The combined keyword `break!` means to leave the scope which is the one most close to the break statement.

**The scope without brackets will be skipped when detecting scopes to break.**

Example:

~~~
if( condition ) {
    execute_something();
    if( not_going_on ) break!;
    something_going_on();
}
~~~

## Break or continue one block after doing something

If you want to break or continue a block, but there's something has to be done before that, use the promise `after` to combine them to be one statement.

Example:

~~~
loop i on data_set : {
    do_something();
    if( condition_to_break )
        break after clean();
}
~~~