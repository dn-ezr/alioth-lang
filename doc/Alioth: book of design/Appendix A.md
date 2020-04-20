---
title: 'Appendix A: Lexical Design'
author: GodGnidoc
date: 2019/08/25
---

# Appendix A: Lexical Design

In this chapter, we'll talk about in which way the lexical rules were designed.

- [Appendix A: Lexical Design](#appendix-a-lexical-design)
  - [Reserved keywords](#reserved-keywords)
  - [Reserved special literal value](#reserved-special-literal-value)
  - [Supported operators](#supported-operators)
  - [Combined keywords](#combined-keywords)
    - [This module](#this-module)
    - [$ return](#return)
    - [This class](#this-class)
  - [Promise keywords](#promise-keywords)
    - [abstract](#abstract)
    - [public private protected](#public-private-protected)
    - [rev ism prefix suffix](#rev-ism-prefix-suffix)
    - [atomic](#atomic)
    - [raw](#raw)
    - [loops](#loops)
      - [infinite loop](#infinite-loop)
      - [condition loop](#condition-loop)
      - [suffixed-condition loop](#suffixed-condition-loop)
      - [step loop](#step-loop)
      - [iterating loop](#iterating-loop)
    - [Execution-stream unwinding](#execution-stream-unwinding)
    - [Hook return process](#hook-return-process)
  - [Lexical patterns](#lexical-patterns)
    - [Basic regular expressions](#basic-regular-expressions)
    - [label](#label)
    - [literal string](#literal-string)
    - [literal character](#literal-character)
    - [literal digit](#literal-digit)

## Reserved keywords

There are series `reserved keyword`s designed to construct syntax structures.

`and`,`as`,`asm`,`assume`,`bool`,`break`,`case`,`class`,`const`,`continue`,`default`,`delete`,`do`,`else`,`entry`,`enum`,`float32`,`float64`,`if`,`int16`,`int32`,`int64`,`int8`,`loop`,`member`,`meta`,`method`,`module`,`new`,`not`,`obj`,`operator`,`or`,`otherwise`,`ptr`,`ref`,`rel`,`return`,`switch`,`as!`,`uint16`,`uint32`,`uint64`,`uint8`,`void`

## Reserved special literal value

There are series of special literal values, compiler reserved keywords for them.

`false`,`null`,`this`,`true`

## Supported operators

Also, there are series operators designed for use.

`=`,`/=`,`-=`,`%=`,`*=`,`+=`,`<<=`,`>>=`,`&=`,`|=`,`^=`,`&`,`|`,`~`,`^`,`)`,`]`,`}`,`(`,`[`,`{`,`:`,`,`,`;`,`?`,`@`,`$`,`--`,`/`,`==`,`...`,`!!`,`!`,`>=`,`>`,`++`,`<=`,`<`,`-`,`%`,`*`,`!=`,`+`,`..`,`::`,`#`,`<<`,`>>`,`^`

## Combined keywords

Some combined keywords are designed.

`this module`,`$ return`,`this class`

### This module

Combined keyword `this module` is a self-reference for module when describing dependencies.

~~~
module Hello :
  net @ alioth as this module
~~~

### $ return

Combined keyword `$ return` is used to refer to the return object before returning from a method.

### This class

The combined keyword `this class` is used to refer to the data type which this class represents.

## Promise keywords

Promise keywords are designed to be available in certain context only.

`abstract`,`public`,`private`,`protected`,`rev`,`ism`,`prefix`,`suffix`,`atomic`,`raw`,`on`,`then`,`while`,`for`.

### abstract

 When defining, the promise `abstract` is used to specify that one class is an abstract class.

~~~
class abstrcat AbstractCompiler {

}
~~~

### public private protected

The visibility control keywords are designed to be promises, because they are used only when defining `classes`,`enums`,`methods`,`operators` and `attributes`.

~~~
class Compiler {
    obj private diagnostics Diagnostics
}
~~~

These three promises are replacreable with three corresponding symbols.

|promise|symbol|
|:-:|:-:|
|`public`|`+`|
|`protected`|`*`|
|`private`|`-`|

~~~
class Compiler {
    obj - diagnostics Diagnostics
}
~~~

### rev ism prefix suffix

The opertor modifiers are designed to be promises.

~~~
class iterator {
    operator prefix ++ ()
    operator suffix ++ ()
}
~~~

### atomic

The method modifier `atomic` is designed to specify that one method is an atomic method which means that this method will not be interrupted. 

~~~
class vector {
    method atomic times( obj an vector ) vector
}
~~~

### raw

The method modifier `raw` is used to change the binary symbol generated for this method. One keyword `raw` means that use the method name as the symbol. A keyword `raw` with a string right following it means that use that string specified as the symbol.

~~~
class Stdlib {
    method raw printf( obj fmt string, args... ) int32
    method raw 'println' pln( obj fmt string, args... ) int32
}
~~~

### loops

There five kinds of loop structures.

#### infinite loop

~~~
loop: /* do something */
~~~

Construct infinite loop using this format, compiler will give up to compile the following instructions.

#### condition loop

~~~
loop while( /* condition */ ) /* do something */
~~~

Construct loop with conditions using this format.

#### suffixed-condition loop

~~~
loop do /* do something */ while( /* condition */ );
~~~

Use this format, the loop body will be executed at least one time.

#### step loop

~~~
loop for( /* init */; /* condition */; /* control */ ) /* do something */
~~~

To construct step loop, use this format.

#### iterating loop

~~~
loop /* iterator */ on /* container */ do /* do something */
~~~

This format is used to iterate containers.

### Execution-stream unwinding

~~~
do /*Proc*/ on /*Task*/ then /*Callback*/;
~~~

### Hook return process

~~~
do return : /*return process*/
~~~

## Lexical patterns

Other one-id-multiple-instance lexical vocabularies are defined by formal regular expressions.

### Basic regular expressions

~~~ebnf
lower letter = 'a'|'b'|'c'|'d'|'e'|'f'|'g'|'h'|'i'|'j'|'k'|'l'|'m'|'n'|'o'|'p'|'q'|'r'|'s'|'t'|'u'|'v'|'w'|'x'|'y'|'z';

upper letter = 'A'|'B'|'C'|'D'|'E'|'F'|'G'|'H'|'I'|'J'|'K'|'L'|'M'|'N'|'O'|'P'|'Q'|'R'|'S'|'T'|'U'|'V'|'W'|'X'|'Y'|'Z';

letter = lower letter | upper letter;

binary digit =  '0'|'1';

octal digit = binary digit|'2'|'3'|'4'|'5'|'6'|'7';

decimal digit = octal digit|'8'|'9';

hex digit = decimal digit|'a'|'b'|'c'|'d'|'e'|'f'|'A'|'B'|'C'|'D'|'E'|'F';
~~~

### label

~~~ebnf
label = letter (letter|decimal digit|'_')*;
~~~

Pattern `label` is used to define how the names or labels in alioth are written.

### literal string

### literal character

### literal digit