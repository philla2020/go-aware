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
```go
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

***A Go application might have one or more packages***. A single package could be one file or more file that at the beginning start
all with the same package declaration. See how to use your own package later on in the documentation.

### Packages, what they are and how to use

Some key point to better understand packages:

- a package is declared using the corresponding keyword
- the source code of a package resides in one o more .go files, located all in the same folder
- usually the name of the package is the name of the folder which is located
- all the files of the same package can use functions and variables without qualifying them
- outside the package to use variables or functions we need point to the package (only for variable or function with the first capital letter)

Ok, try to create a new simple project with this structure:
```
├── main.go
└── pkgtest
    └── test.go
```
The content of the file `main.go` is:
```golang
package main

import (
	"fmt"
	"pkgtest"
)

func main(){
  fmt.Println("Start...")
  fmt.Println(pkgtest.Version())
}
```
and the content of the file `test.go` is:
```golang
package pkgtest

func Version() string {
	return "1.0.0"
}
```

we have a project executable due to the presence of the main func and we have a nested package to use, named *pkgtest*.
We know that to export something the first letter could be capitalize so we try:
```
go run main.go
```
but, unfortunately it doesn't work...let's take a look:
```
go run main.go 
main.go:5:2: cannot find package "pkgtest" in any of:
        /usr/local/Cellar/go/1.14.2_1/libexec/src/pkgtest (from $GOROOT)
        /Users/andrea/go/src/pkgtest (from $GOPATH)
```
Ops...Go is trying to fetch the source of pkgtest for two locations: GOROOT (where go is installed) and GOPATH. Both cases the searched folder is **`src`**.

The Go versions >1.11 solve this issue by the **module** feature but we see later on this, so the only way to get work the project is to
move it from the current location (wherever it is) to the GOPATH/src folder.
The folder to move is only pkgtest, the rest of the project can be wherever you want. Therefore, copy the pkgtest into the GOPATH/src directory.
In this example we move here:

```shell
/Users/andrea/go/src/pkgtest
```

Let's try `go run main.go`  and it will work.

#### Packages initialization

If you have more than one file that refer to the same package, they are initialized in the order where are fetched from the compiler (alphabetical order).
If you need to group some init steps you can use the ***init()*** func that is automatically invoked from Go. The dependencies are initialized first to be sure the corresponding package is ready before using it.

**Main package** is the last to be initialized.

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



### Variables scope

Inside a Go program the variables or constants have visibility at:

- `package level`: each variable or constant define into a package is visibile inside the files belong to that package and from external package if the name is with a capital letter.
- `function level`: variables are visible inside the scope of the function. There are some exceptions but are more advanced concepts, for now focus on this principle.
- `block level`: for block we refer to statements enclosed by brackets {}. For instance: for loop, if, switch etc... The variable declared into a block lives into it.

### Assignment

Simple assignment in Go comes with the form `var = value`. You can also have:

```go
// declaration and assignemnt
i := 1

// increment by one
i++
// decrement by one
i--
// short form to increment by x
i += 5
// it is the same as
i = i + 5
```

Another particular form of assignment is the ***Tuple assignment***. This assignemnt is useful to swap value of the variable:
```
a, b = b, a
```

a common use of this assignment is when we invoke a function that returns back more than one value:

```
err, val := someFunc(x)
```

if we do not want to evaluate some variables we can use the *blank identifier* instead:
```
_, val := someFunc(x) // ignore error
```

{{< alert title="Tip" color="success" >}}
To assign a value the variable and the value must have the same type, or better the assignment is valid only if a specific value is ***assignable*** to a specific type.
{{< /alert >}}


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

### Create your own type

It means to create a specific type that has the same type of the underlying one. This is useful to better represent a variable whene the Go type could be not so specific. For example `int` type can be used for meters, miles, age etc...

The way to define a specific type is:

*type* **\<name\>** **\<underlying-type\>**

Let's see an example:
```go
package main

import (
	"fmt"
)

type Meters float64
type Miles float64

func main() {
	var a Meters = 1000
	var b Miles = Miles(a) * Miles(0.621371)
	fmt.Println("a, b are:", a, b)
}
```

{{< alert title="Tip" color="success" >}}
A type conversion is possible if the two types have the same underlying value or are both pointers to the same object. In case you convert between
different types Go could make a truncation of the value or a copy of them (string => byte slice). For instance:
```go
var b Miles = Miles(a) * Miles(0.621371)
var c int = int(b) // value will be 621
```
Remember that in any case will be an error at runtime.
{{< /alert >}}
Two different variables can be compared if belong to the same type or can be compared with an underlying type. A comparison 
between two different type will fail at compilation time. For instance:
```go
fmt.Println("a == 0", a == 0)
fmt.Println("a > 0", a > 0)
fmt.Println("a == b (Meter == Miles)", a == b) // will not build
```

### Integers and Float [TODO]

### Complex numbers [TODO]

### Booleans [TODO]

### String [TODO]

### Constants [TODO]