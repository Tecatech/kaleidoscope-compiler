# Kaleidoscope: Implementing a Parser and AST

## Introduction

This chapter shows you how to use the lexer to build a full `parser` for our Kaleidoscope language. Once we have a parser, we’ll define and build an `Abstract Syntax Tree` (AST).

The parser we will build uses a combination of `Recursive Descent Parsing` and `Operator-Precedence Parsing` to parse the Kaleidoscope language (the latter for binary expressions and the former for everything else). Before we get to parsing though, let’s talk about the output of the parser: the Abstract Syntax Tree.

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