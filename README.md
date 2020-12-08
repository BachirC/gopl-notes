# gopl-notes
Notes about the book "Go Programming Language"

## History
- C gave Go most of its syntax and compilation strategy
- Oberon-2 (Pascal's successor) inspired the syntax for packages, imports, declarations, method declarations.
- Communicating Sequential Processes (CSP) gave Go its concurrency model.

## Tutorial
- Package main is special. It defines a standalone executable program, not a library. Within
package main the function main is also special — it’s where execution of the program begins.
Whatever main does is what the program does.
- Opening brackets `{` must go on the same line as the declaration that preceeds it for some particular tokens (e.g. : after `func` or `if` declaration)
