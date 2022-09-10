# Kaleidoscope: Implementing a Parser and AST

## Introduction

This chapter shows you how to use the lexer to build a full `parser` for our Kaleidoscope language. Once we have a parser, we’ll define and build an `Abstract Syntax Tree` (AST).

The parser we will build uses a combination of `Recursive Descent Parsing` and `Operator-Precedence Parsing` to parse the Kaleidoscope language (the latter for binary expressions and the former for everything else). Before we get to parsing though, let’s talk about the output of the parser: the Abstract Syntax Tree.

## The Abstract Syntax Tree (AST)

The AST for a program captures its behavior in such a way that it is easy for later stages of the compiler (e.g. code generation) to interpret. We basically want one object for each construct in the language, and the AST should closely model the language. In Kaleidoscope, we have expressions, a prototype, and a function object.

![alt text](https://github.com/tecatech/kaleidoscope-compiler/blob/main/01-Implementing_a_Parser_and_AST/images/clang_llvm_abstract_syntax_tree.png)

In our example, functions are typed with just a count of their arguments. Since all values are double precision floating point, the type of each argument doesn’t need to be stored anywhere.

With this scaffolding, we can now talk about parsing expressions and function bodies in Kaleidoscope.

## Parser Basics

Now that we have an AST to build, we need to define the parser code to build it. The idea here is that we want to parse something like `x + y` (which is returned as three tokens by the lexer) into an AST that could be generated with calls like this:

![alt text](https://github.com/tecatech/kaleidoscope-compiler/blob/main/01-Implementing_a_Parser_and_AST/images/clang_llvm_parser.jpg)

The error recovery in our parser will not be the best and is not particular user-friendly, but it will be enough for our tutorial. These routines make it easier to handle errors in routines that have various return types: they always return null.

## Conclusions

Because our compiler uses the LLVM libraries, we need to link them in. To do this, we use the `llvm-config` tool to inform our command line about which options to use:

```
$ clang++ -g -O3 main.cpp `llvm-config --cxxflags`
$ ./a.out
kaleidoscope >>> def foo(x y) x + foo(y, 4.0);
Parsed a function definition
kaleidoscope >>> def foo(x y) x + y y;
Parsed a function definition
Parsed a top-level expression
kaleidoscope >>> def foo(x y) x + y );
Parsed a function definition
Error: unknown token when expecting an expression
kaleidoscope >>> extern sin(a);
Parsed an extern
kaleidoscope >>> ^D
```