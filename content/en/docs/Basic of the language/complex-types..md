---
title: "Complex Types"
linkTitle: "Complex Types"
description: >
  This paragraph is to explain some complex types that you will use on a daily basis.
---

Explanation of the page.

## Arrays

Arrays are fixed collections of a declared type.

Some key points to remember are:

- are fixed and cannot resize
- the array elements can be modified
- are pointed started from zero to the lenght minus one
- by default the elements of a new array are initialized to the ZERO values for that type 
- we can use literals to initialize an array
- it is possible to **compare only two arrays of the same type and the same length**
- two arrays are equals if all the elements have the same values

{{% alert title="Important" color="warning" %}}
Remember that every time you pass an array to a func Go is copying the array into another one, it is time and memory consuming.
Also in this case you can use **pointers**.
{{% /alert %}}

This is an example that covers all the topics listed above (printValues is a function that print simply a value):

```golang
var a [5]string // declaration of an array of 5 elements (init at ZERO values)
// cycle to get the values
printValues(a)
// set first and last value
a[0] = "first"
a[len(a) - 1] = "last"
printValues(a)

// initialization using literals
var b [5]string = [5]string{"a", "b", "c", "d", "e"}
//	b[5] = "test" // compile error => out of bounds
printValues(b)

// initialization using literals (without var) taking the length from the
// number of elements
c := [...]string{"a", "b", "c", "d", "e"}
printValues(c)

// it is possible to compare only two arrays of the same type and the same length
// two arrays are equals if the elements have the same values
fmt.Println("b == c", b == c)

// init and implicit declare specifying the size
i1 := [3]int{1,2,3}
i2 := [3]int{1,2,4}
i3 := [4]int{1,2,4,5}
fmt.Println("i1 == i2", i1 == i2)
fmt.Println("i3:", i3)
//	fmt.Println("i1 == i3", i1 == i3) // compile time error, same type but different length
```

## Slices [...]
Slices are fixed collections of a declared type.

{{< alert title="Keypoints" color="success" >}}
- declaration is similar to the array without having to declare the length
- it has three components a pointer, a length, a capacity
- is a ligthweigth structure that gives access to the underlying structure that is an array
- **pointer**: the first element reachable of the array (not necessarily the first)
- the **length** is the number of slice elements (***len*** function gives it)
- the **capacity** is the number of elements between the start of the slice and the end of the array (***cap*** function gives it)
- multiple slices can refer to the same underlying array
- you cannot use == operator to test if two slices have the same values, you need to do your own func
{{< /alert >}}
Follow an awesome image taken from the book ***Go Programming Language*** that illustrates the len, cap and underlying array:

![](/img/slices.png)

The operator **x[i:z]** returns a new slice that goes from i (int index) to z -1 of a sequence z. What is z?
- another slice
- an array
- a pointer to an array
if i is not present it is 0, if z is not present, it assumes len(x):

```golang
months := []string{1: "January", 3: "March", "April", "May", "June", "July"}
// an example of the slice operator that creates a new slice
firstMonths := months[0:3] // -> 0,1,2 indexes
printValues(firstMonths)
// other slice examples
allMonths := months[:]
// from beginning to end
allMonthsTwo := months[0:]
// or
allMonthsThree := months[0:9]
printValues(allMonths)
printValues(allMonthsTwo)
printValues(allMonthsThree)
```

Knowing the existence of the underlying array we can extend a slice inside the cap of the array, referring to the previous
example:

```golang
// to extend a slice always inside the capacity
fourMonths := firstMonths[:4]
printValues(fourMonths)
```
Slice is a ***reference type*** because it contains a pointer to an array, therefore passing a slice as an argument to a function permits to modify the underlying array. Remember that Go always copy the value but, in this case, it copies a pointer not an entire structure.
To show this take a look at the following reverse example, as you can notice the argument passed to the function is the original slice i
that after execution is reversed:

```golang
// int slice and reverse (Go passes a copy of a pointer of the slice, so the func can change the values)
i := []int{1,2,3,4,5,6}
reverse(i)
fmt.Println(i)

  // Reverse a slice
func reverse(a []int){
	for i,j := 0, len(a) - 1; i < j; i,j = i+1, j-1{
		a[i], a[j] = a[j], a [i]
	}
}
```
in the above example remember the multiple assingment separated by comma, *i,j := 0, len(a) - 1*, where i is 0 and j is 5.

The **ZERO value** of a slice type is nil:
```golang
// nil values
var s []int
fmt.Printf("len(s) is %v, len == nil; %v\n", len(s), s == nil) // len(s) is 0, len == nil; true
s = nil
fmt.Printf("len(s) is %v, len == nil; %v\n", len(s), s == nil) // len(s) is 0, len == nil; true
s = []int(nil)
fmt.Printf("len(s) is %v, len == nil; %v\n", len(s), s == nil) // len(s) is 0, len == nil; true
s = []int{}
fmt.Printf("len(s) is %v, len == nil; %v\n", len(s), s == nil) // len(s) is 0, len == nil; false
```

> Creation of a slice with **make**
```golang
// creation with make (create a variable with all the elements of ZERO types each one)
b := make([]int, 10, 20)
fmt.Println(b) // => [0 0 0 0 0 0 0 0 0 0]
```
Behind the scene, make creates and underlying unnamed array variable and return a slice of it. The array is accessible only throghout
the slice.

### Append new element
Use the built-in **append** function:
```golang

```
{{< alert color="warning" title="ATTENTION" >}}
Every time a slice changes its ***cap*** means new allocation and copy, therefore pay attention to the cap and len it's a good
way to be more efficient.
{{< /alert >}}


### [I AM HERE] take a look at the the mark on kindle

## Struct

It is simply a collection of fields/properties.
In an OOP perspective we can think of a struct to be a light class that supports composition but not inheritance.

{{% alert title="Important" color="warning" %}}
Outside a package where the Struct is located can be accessed only the capitalized fields.
{{% /alert %}}

### Init a struct

Suppose to have the struct:

```go
// Simple struct
type vehicle struct {
  brand      string
  model      string
  horsepower int
  year       int
}
```

it can be initialized as:

```go
// 1- Using the literal syntax
fmt.Println(Vehicle{brand: "Fiat",
  model:      "Punto",
  horsepower: 90,
  year:       2002})
 
// 2- Using the the dot notation
var v vehicle
v.brand = "Tesla"
v.model = "Tesla1"
v.horsepower = 120
v.year = 2020
fmt.Println(v)
 
// 3- using literal in a particular order omitting the name of the field
fmt.Println(vehicle{"Fiat", "Giulia", 140, 2018})
 
// 4- using the new keyword that creates a pointer to the type initialing the fields
// with the zero value of the specific type
fmt.Println(new(vehicle))
// It is similar to
fmt.Println(&vehicle{})
```
For the moment don't take care at the '&' character in the last row.

### Add a method to a structure

To add a method you have to use a receiver that is a special way that Go passes at the method the value of the object related to the strut itself.
To create a method you have this special syntax:

```go
func (varName Type) Method()  {
	...
}
```

for example, if you want to add a method Go to the vehicle struct, type:

```go
func (v Vehicle) Go() {
	fmt.Println("Started!")
}

// and use it
var v Vehicle
v.brand = "Tesla"
v.model = "Tesla1"
v.horsepower = 120
v.year = 2020
fmt.Println(v)
v.Go()
```
Under the hood, Go will take the v object and passes its instance to the Go() function, so inside the method you can use v object.
**Remember** that Go will pass the value of the v object, therefore if you change it (for instance changing a value of a field) inside the method, the original object is still unchanged.

### Type composition

What is this technique? You can use to compose two or more structures together "similar" as an OOP approach for inheritance.

For example, suppose we have this two type, Basic and Specific:

```go
type BaseType struct {
  Name string
  Surname string
  Age int
}
 
/*
Specific type contains the BaseType struct
*/
type SpecificType struct {
  BaseType
  Type string
  Category string
}

```

As you can see the first property of the SpecificType struct is another Structure and we not specify the property name. By this way you can init the SpecificType using the dot notation as:

```go
var st SpecificType
st.Name = "My name" // BasicType
st.Age = 12 // BasicType
st.Category = "Cat of spec type"
st.Type = "Type of spec type"
st.Surname = "Surname" // BasicType
```

instead, using the literal form, we must respect the order of the fields:

```go
st1 := SpecificType{
  BaseType{Name:"My name", Surname: "Surname from Base Type", Age: 12},
  "Type of spec type",
  "Cat of spec type",
}
```

or, we have to specify the fields name:

```go
st1 := SpecificType{
  Type: "Type of spec type",
  Category: "Cat of spec type",
  BaseType: &BaseType{Name:"My name", Surname: "Surname from Base Type", Age: 12},
}
```

{{% alert title="Note" color="info" %}}
If you define a method on a Struct and there is a composition, the method itself is available on the struct that includes another struct. **Remember** that this is valid only for the implicit composition as seen above. Instead, if you assign the structure type to a field, for instance:

```go
type Job struct {
  Command string
  log *log.Logger
}
```

you cannot invoke the Logger method inside Job object directly, you can only call using:

```go
var j Job
j.log.info()
```

{{% /alert %}}

{{% alert title="Tip" %}}
It is better to have a pointer in a composition for performance reasons. Go “always” passes the argument by copying values, using a pointer only the reference is passed. For instance:

```go
type SpecificType struct {
*BaseType // using a pointer is better for performance reason
Type string
Category string
}
```

By this way you cannot use the dot notation to init the object because the pointer would be nil, you can only use this way:

```go
st1 := SpecificType{
&BaseType{Name:"My name", Surname: "Surname from Base Type", Age: 12},
"Type of spec type",
"Cat of spec type",
}
```
For major information related to the use of Pointers take a look [here](../pointers/)
{{% /alert %}}







## Maps [TODO]

## JSON [TODO]

