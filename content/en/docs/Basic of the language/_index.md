---
title: "Basic of the language"
linkTitle: "Basic of the language"
weight: 2
description: >
  What does you need to know to try with Golang?
---

This chapter show the basic knowldge to have to work comfortably with Golang.

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
