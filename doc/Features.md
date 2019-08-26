---
title: Features
author: GodGnidoc
date: 2019/08/07
---

# Abstract

This short document is used to describe all features imported since the Alioth programming language was designed.

- [Abstract](#abstract)
- [Language Promise](#language-promise)
  - [Lexical Promise](#lexical-promise)

# Language Promise

The mechanism `Language Promise` provides way to associate abstract concepts using context but not syntax structure.

## Lexical Promise

The `Lexical Promise` is a mechanism help extending vocabulary in certain context.

Some modifier was defined in other programming languages to specify features of syntax structures, such as `abstract`, `atomic`,`public` and so on.

These modifiers are used just in some certain context, but they always be scaned as keywords. We think that causes the limition of freedom when naming abstract units. Thus, the mechanism `lexical promise` was intrduced, tokens like that will be scaned to have special meaning only in certain context.