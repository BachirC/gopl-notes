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
- `i++` is a statement, not an expression -> `j = i++` is illegal. _Check diff statement/expression ?_
 - Printf has overadozen such conversions, which Go programmers call verbs.

|verb       | type                                    |
|-----------|-------------------------------------------
| %d         | decimal integer                        |
| %x, %o, %b | integer in hexadecimal, octal, binary  |
| %f, %g, %e | floating-point number: 3.141593        |
| %t         | boolean: true or false                 |
| %c         | rune (Unicode code point)              |
| %s         | string                                 |
| %q         | quoted string "abc" or rune 'c'        |
| %v         | any value in a natural format          |
| %T         | type of any value                      |
| %%         | literal percent sign (no operand)      |

**Variable declaration**
```go
s := ""
var s string
var s = ""
var s string = ""
```
Why should you prefer one form to another? The first form, a short variable declaration, is the most compact, but it may be used only within a function, not for package-level variables. The second form relies on default initialization to the zero value for strings, which is "". The third form is rarely used except when declaring multiple variables. The fourth form is explicit about the variable’s type, which is redundant when it is the same as that of the initial value but necessary in other cases where they are not of the same type. In practice, you should generally use one of the first two forms, with explicit initialization to say that the initial value is important and implicit initialization to say that the initial value doesn’t matter.

**Reading files**
  - (Stream) https://pkg.go.dev/bufio#NewScanner : Read input and breaks it into lines or words. Call `Scan()` on return value to read a line and delete it. Scan returns false when no more lines to be read. Use `Text()` to get the content.
  - (One-go) https://pkg.go.dev/io/ioutil#ReadFile : Reads whole input in memory and returns it.
  - (One-go) https://pkg.go.dev/io/ioutil#ReadAll : `ReadFile` but for any io.Reader.
  - (Stream) https://pkg.go.dev/io#Copy : Copy content from dst to src.

**Concurrency**
  - A goroutine is a concurrent function execution. A channel is a communication mechanism that allows one goroutine to pass values of a specified type to another goroutine.
  - When one goroutine attempts a send or receive on a channel, it blocks until another goroutine attempts the corresponding receive or send operation, at which point the value is transferred and both goroutines proceed.
  - https://pkg.go.dev/sync#Mutex : Mutex for goroutines.

**Web servers**
  - https://pkg.go.dev/net/http : std package for creating web servers.
  - https://pkg.go.dev/net/url#URL.Query : Get query params.

**Pointers**
  - Explicitly visible.
  - No pointers arithmetic.
  
## Structure

**Declarations**
- package-level declarations are visible throughout all files of the package.

**Variables**
- Omitting the type in variable declaration => takes type of initialization value.
- Omitting the initialization value => Take 0 value of its type.

| type | num |
-------|-------
| number | 0   |
| boolean | false |
| strings | "" |
| interfaces and reference types | nil |
| aggregate types | zero values of each element/field |

- Package-level variables are initialized before main begins, and local variables are initialized as their declarations are encountered during function execution.
- `new(T)` expression that creates an unnamed variable of type T initialized to the zero value and returns its address.

**Type declaration**
- Use

```golang
type Celsius float64
type Fahrenheit float64
```

to avoid comparing two variables that have the same underlying type (here float64) but that represent two semantically different things (Celsius and Fahrenheit).

- T(x) converts x to type T. A conversion from one type to another is allowed if both have the same underlying type, or if both are unnamed pointer types that point to variables of the same underlying type; these conversions change the type but not the representation of the value.
- Conversions are also allowed between numeric types, and between string and some slice types, as we will see in the next chapter. These conversions may change the representation of the value (3.14 -> 3, string -> `[]byte` slice allocates a copy of the string data).

**Imports**
- Each package is uniquely identified by its **import path**.
- How to use go.mod with local packages https://forum.golangbridge.org/t/cannot-use-custom-package/18226/4

- `bytes` type is an alias of `uint8`.
