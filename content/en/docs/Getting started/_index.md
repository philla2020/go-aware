---
title: "Getting Started"
linkTitle: "Getting Started"
weight: 1
description: >
  What does your user need to know to try use Golang?
---


## Prerequisites

To use Go you need to install all the software and the use some IDE you prefer (e.g. **Visual Studio Code**, **Goland** or simply you favourite terminal).

## Installation

The first place to go to get started with `Golang` is the official [site](https://golang.org/doc/install).

### Mac OSX

The official site suggests to download the package but an alternative (and valid) way is using **`HomeBrew`**. If have not installed on your machine you can get from [here](https://brew.sh/index_it).
Then to install `Go` you can type:

```
$ brew install go
```

### Linux

The suggestion is to directly download the last version of Golang and untar it into the local folder.
{{< alert color="warning" >}}If you have a previous version **ensure to uninstall** it before proceeding.{{< /alert >}}

* Download the archive and extract it into /usr/local:

```
$ tar -C /usr/local -xzf go<VERSION>.linux-amd64.tar.gz
```

* Add the version to *your local path* (if you already have it, jump this step):
  
```
export PATH=$PATH:/usr/local/go/bin
```

The file where to insert the above export instruction, depends from your system and shell.
For example if you have Ubuntu installation you can change the file `$HOME/.profile`.

### Linux with a package manager

The best option in this case is referring to the documentation of your favourite distro. For example, if you are on **Ubuntu** you can type:
```
$ sudo apt install golang-go
```
or, if you have a system with `snap` installed:
```
snap install go
```

## Try it out!

Regardless of the method you chose, to check that the installation is working, try this command:
```
$ go version
```
The output should look like something as:
```
go version go1.15.5 darwin/amd64
```