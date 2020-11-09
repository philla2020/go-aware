---
title: "Using Functions"
linkTitle: "Using Functions"
description: >
  What they are and how to use them
---

{{% pageinfo %}}
Work in progress...
{{% /pageinfo %}}

## Interesting topics

Before starting to explore the functions it is worth to mention some interesting concepts subject to misunderstanding.

### Parameters vs Arguments
- **Parameters** are variables declared into a Function declaration. They are the variables that we can use into the Function itself.
- **Arguments** are the values that are passed to the Function when invoking it.

### Expressions vs Statements
In IT the two concepts are often confused.
- **expression** is whatever you can assign or print. We can see as something that returns a value.
```golang
5 * 5
len(y)
max(x)
print(5)
```
- **statement** is an istruction executed inside a process:
```golang
if .. else
for .. := range xx
while {}
```

## A simple Function
A function declaration has a **name**, a **list of parameters**, an **optional list of results**, and a **body**:
```golang
func deleteElements(m map[string]int) {
	delete(m, "ag")
	fmt.Printf("values of m are %v\n", m)
}
```
In the above example:
- `deleteElements` is the name
- `m` is the parameter of type `map[string]int`
- we have nothing in return
- we have a body with two statements

the structure is:
```
func name(parameters) ret-values {
  body
}
```
- `parameters` can be more than one, separated by comma (name + type). Arguments are passed ***by value***: Go copies the value of the argument. In general the changes to the parameters do not affect the local variable passed as argument (except for reference types or if we use a pointer).
- `ret-values` can be more than one, separated by comma (type). It is possible to have names return types but is not often used.
- `body`: the brain of a function. It could be a function without, it one implemented in a language other than Go

{{< alert color="success" title="Note" >}}
Comparing to Python, **Go has not**:
- *named arguments* (only positional ones)
- way to set *default values* for the parameters
{{< /alert >}}

the ***type*** of a function is called *signature*: it is the type of the parameters and the type of the return values.
For example for these functions:
```golang
func example(par1 int, par2 string) int {
	fmt.Println("the par2 has this value: ", par2)
	return par1 * 2
}

func exampleNoReturn(par1 int, par2 string) {
	fmt.Println("the par2 has this value: ", par2)
}
```
to get the type (or signature) write:
```golang
fmt.Printf("Type of example is %T\n", example)
fmt.Printf("Type of exampleNoReturn is %T\n", exampleNoReturn)
```
and we get:
```
Type of example is func(int, string) int
Type of exampleNoReturn is func(int, string)
```

## Multiple return values
It is possible to have multiple return values and pick them, for example:
```golang
	// call a function that returns multiple values
	ret1, ret2 := multipleReturns(100, "test")

  /*
	This func has two params (int and string) and returns to result to the
	caller.
*/
func multipleReturns(par1 int, par2 string) (int, string) {
	fmt.Println("the par2 has this value: ", par2)
	return par1 * 2, par2 + "-return"
}
```
To ignore any of the parameters use the '_' character:
```golang
	// call a function that returns multiple values
  ret1, _ := multipleReturns(100, "test")
```
It is possible use named variables as return, instead of single types, for example:
```golang
func multipleNamedReturns(par1 int, par2 string) (val int, name string) {
	fmt.Println("the par2 has this value: ", par2)
	val = par1 * 2
	name = par2 + "-test"
	return
}
```
in the example above we use a technique called `bare return` possible when using named params. It will return all the parameters with the current value.

## Recursion technique
This technique is useful when we need to traverse some content that has a tree structure.
At a first glance could be a terrible exercise for a human and easily for a computer but, with a bit of practice, it will be more readable.

Guess to have this string:
```golang
var webSite = `
<html>
  <head>
    <title>your title here</title>
  </head>
  <body bgcolor="ffffff">
    <center>
      <img src="clouds.jpg" align="bottom">
    </center>
    <hr>
    <a href="http://somegreatsite.com">link name</a>
    is a link to another nifty site
    <h1>this is a header</h1>
    <h2>this is a medium header</h2>
    <a href="http://text2.com">link name</a>
    send me mail at <a href="mailto:support@yourcompany.com">support@yourcompany.com</a>
    <p> this is a new paragraph! </p>
    <p>
      <b>this is a new paragraph!</b>
      <br> <b><i>this is a new sentence without a paragraph break, in bold italics.</i></b>
      <hr>
    </p>
  </body>
</html>`
```
This is a simple webpage. Using the html package we will search all the tag of type <a> using recursion. Remember that HTML is a tree, so html is the first leaf, head and body are siblings and so on.
Write the search func:
```golang
func searchHtml() {
	doc, err := html.Parse(strings.NewReader(webSite))
	if err != nil {
		fmt.Fprintf(os.Stderr, "findlinks1: %v\n", err)
		os.Exit(1)
	}
	for _, link := range visit(nil, doc) {
		fmt.Println(link)
	}
}
```
We do it:
- parse HTML
- call the visit that will get a link []string
The `visit` is our recursive function that will call itself when:
- the html node has a child or a sibling
This is the function:
```golang
func visit(links []string, n *html.Node) []string {
	if n.Type == html.ElementNode && n.Data == "a" {
		for _, a := range n.Attr {
			if a.Key == "href" {
				links = append(links, a.Val)
			}
		}
	}
	for c := n.FirstChild; c != nil; c = c.NextSibling {
		links = visit(links, c)
	}

	return links
}
```
By this way there is a recursion, for simplcity add a print to understand on which node is the visit func:
```golang
func visit(links []string, n *html.Node) []string {
	if n.Type == html.ElementNode && len(n.Parent.Data) > 0{
		fmt.Printf("%s.%s -> ",n.Parent.Data, n.Data)
	}
	if n.Type == html.ElementNode && n.Data == "a" {
		for _, a := range n.Attr {
			if a.Key == "href" {
				links = append(links, a.Val)
			}
		}
	}

	for c := n.FirstChild; c != nil; c = c.NextSibling {
		links = visit(links, c)
	}

	return links
}
```
This output will help to better understand:
```
html.head -> head.title -> html.body -> body.center -> center.img -> body.hr -> body.a -> body.h1 -> body.h2 -> body.a -> body.a -> body.p -> body.p -> p.b -> p.br -> p.b -> b.i -> body.hr -> body.p ->
http://somegreatsite.com
http://text2.com
mailto:support@yourcompany.com
```

## Functions as values

Like all the other type, functions have a value, its own value types and can be assigned to a variable:
```golang
func example(par1 int, par2 string) int {
	fmt.Println("the par2 has this value: ", par2)
	return par1 * 2
}

// assign function to a variable
f := example
// then call it
fmt.Println("result of f() is:", f(1, "10"))
```
- type of a function are parameters and return values
- value of a function is the body
- the ***ZERO value*** of a function type is nil.
