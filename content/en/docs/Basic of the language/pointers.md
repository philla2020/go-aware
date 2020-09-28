---
title: "Using Pointers"
linkTitle: "Using Pointers"
description: >
  What they are and how to use them
resources:
- src: "**create*.jpg"
  params:
    byline: "Photo: Bjørn Erik Pedersen / CC-BY-SA"
---

{{% pageinfo %}}
***Work in progress***
{{% /pageinfo %}}

{{< imgproc create Fill "400x450" >}}
Norway Spruce Picea abies shoot with foliage buds.
{{< /imgproc >}}

**What is a pointer?**

A pointer is an address that references some area in memory. A pointer variable is a variable that stores that address.
So, using a pointer, we can know and use the address to reference a value.
In Go to know the address of a variable we use the & character and to get the underlying value of a pointer we use * as a prefix for the variable name.
For example:
