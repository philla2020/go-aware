---
title: "Basic of the language"
linkTitle: "Basic of the language"
weight: 2
description: >
  What does you need to know to try with Golang?
---

This chapter show the basic knowldge to have to work comfortably with Golang.

## Structure of a Go application

In Go any program is composed by one or many files that end with '*.go'. Every file has a `package` declaration on top it.
The elements are:
- **package** declaration
- import declaration
- declaration of types, variables, constant and functions (order doesn't matter)

An example of a Go application:
```golang
package main

import "fmt"

type BaseType struct {
	Name    string
	Surname string
	Age     int
}

func (b *BaseType) Greetings() {
	fmt.Println("Hello from BaseType!")
	(*b).Name = "test"
}

/*
Specific type contains the BaseType struct
*/
type SpecificType struct {
	*BaseType // using a pointer is better for performance reason
	Type      string
}

func createSpecificType() {
	// new SpecificType using literal
	st1 := SpecificType{
		Type:     "Type of spec type",
	}
	st1.Greetings()
}
```
In this example we have in order: package declaration, import declaration, types and functions.

***A Go application might have one or more packages***. A single package could be one file or more file that at the beginning start all with the same package declaration. See how to use your own package later on in the documentation.

### Variables scope

Inside a Go program the variables or constants have visibility at:

- `package level`: each variable or constant define into a package is visibile inside the files belong to that package and from external package if the name is with a capital letter.
- `function level`: variables are visible inside the scope of the function. There are some exceptions but are more advanced concepts, for now focuse on this principle.

## Variables

In Go a variable can be declared in various way. It can be declared and then assigned or both at the same time.
Take a look at some examples.

### Long variable declaration

Using the *var* keyword with this syntax:

**var** name ***type*** = ***value*** ==> type and value can be omitted but not both, it means that:
- if you omit type the type of the variable is set by the value
- if you omit the value the variable is initialized with the ***ZERO*** value that is:
  - 0 for numbers, false for booleans, "" for string, and **nil** for interfaces and reference types (slice, pointer, map, channel, function). Therefore in Go there is no such thing as uninitialized variables.

Here some basic usage:
```go
// explicit assignment
var (
  name1     string
  age1      int
)
name1, age1 = "test", 1
fmt.Println("variables are:", name1, age1)

// declaration of multiple variables
var a,b,c int
println("variables a,b,c, are:", a,b,c)

// Variable can also be declared one by one
var one string
var second int
var third bool = false
one, second = "hello", 100
fmt.Println("variables are:", one, second, third)
```
### Short variable declaration

In this example name and location are implicit infered as string by Go:

```go
name, location := "Prince Oberyn", "Dorne" // implicit string
test := 100 // implicit int
```
**`IMPORTANT`**: implicit declaration cannot be use at the package level.

## Types

This is the list of the supported types in Golang.

Go can manage these types:
- `bool`
- `string`

Numeric types:
- `uint`        either 32 or 64 bits
- `int`         same size as uint
- `uintptr`     an unsigned integer large enough to store the uninterpreted bits of
              a pointer value
- `uint8`       the set of all unsigned  8-bit integers (0 to 255)
- `uint16`      the set of all unsigned 16-bit integers (0 to 65535)
- `uint32`      the set of all unsigned 32-bit integers (0 to 4294967295)
- `uint64`      the set of all unsigned 64-bit integers (0 to 18446744073709551615)
- `int8`        the set of all signed  8-bit integers (-128 to 127)
- `int16`       the set of all signed 16-bit integers (-32768 to 32767)
- `int32`       the set of all signed 32-bit integers (-2147483648 to 2147483647)
- `int64`       the set of all signed 64-bit integers
              (-9223372036854775808 to 9223372036854775807)
- `float32`     the set of all IEEE-754 32-bit floating-point numbers
- `float64`     the set of all IEEE-754 64-bit floating-point numbers
- `complex64`   the set of all complex numbers with float32 real and imaginary parts
- `complex128`  the set of all complex numbers with float64 real and imaginary parts
- `byte`        alias for uint8
- `rune`        alias for int32 (represents a Unicode code point)

### Type assertion and type conversion

How can you convert a type into another?

To **convert** type you can use the type as you invoked a function, for example:
```golang
var i int = 100
var f float64 = float64(i) // convert int => float64
var u uint = uint(f)       // convert float64 to an unsigned int
to assert a type:
z, ok := y.(map[string]interface{})
if ok {
  z["updated_at"] = time.Now()
  fmt.Println(z)
}else{
  fmt.Println("y is not of the type map[string]interface{}")
}
```
ok is a boolean that is true if the type corresponds to the type passed. If true z will contain the value of the type, else the zero value for that type.
To assert via **`switch`** take a look here:
```golang
// check a type with switch..case
switch str := y.(type) {
case int:
  fmt.Println("int type passed as argument:", str)
case map[string]interface{}:
  fmt.Println("y is map[string]interface{}:", str)
}
```
A fantastic example taken from golangbootcamp.com is:
```go
if err != nil {
  if msqlerr, ok := err.(*mysql.MySQLError); ok && msqlerr.Number == 1062 {
    log.Println("We got a MySQL duplicate :(")
  } else {
    return err
  }
}
```
**mysqlerr** variable can get the value in case of type mysql.MySQLError. In such case ok will be true and, if true, the code will evaluate
its value equals to 1062 printing the corresponding message "We got a MySQL duplicate :(". In this example there is a check, assignment and if into the same row.
