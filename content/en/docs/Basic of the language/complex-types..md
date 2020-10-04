---
title: "Complex Types"
linkTitle: "Complex Types"
description: >
  This paragraph is to explain some complex types that you will use on a daily basis.
---

Explanation of the page.

## Arrays [TODO]

## Slices [TODO]

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

