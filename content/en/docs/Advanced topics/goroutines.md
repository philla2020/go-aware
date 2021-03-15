---
title: "Goroutines"
linkTitle: "Goroutines"
weight: 2
description: >
  How to write a concurrent program?
---

This chapter explains the use of the Goroutines.

## What are the Goroutines?
In ***Golang*** the routines are similar what in other languages is referred as thread.
If you have to execute two or more functions in a concurrent way, goroutine is what is right for you.
Concurrency and parallelism are 2 different topic, I invite you to search on Internet for major information.
In ***Golang*** the `main`program is executed also as a routine, in that case the main routine.

### Your first go routine
To write your first go routine, simply add the `go` keyword to a function call. That function will be executed in another go routine concurrent with the main routine.
{{% alert title="Important" color="warning" %}}
Remember that when the **main** routine ends all the others go routine still in execution are terminated.
{{% /alert %}}


