## Go Tools - Part 1

# Overview

Go tools are the great features that exist alongside the language as part of the `go` command. Almost Go tools come together with Go installation, but there are some you might install depending on your needs.

# Installation

The easiest way to install a Go tool is to run `go get -u golang.org/x/tools/...`. Another way to do it is to `git clone` the repository to `$GOPATH/src/golang.org/x/tools`.

There are some helpful flags:

`-u` forces the tool to sync with the latest version of the repo.

`-d` if you just want to clone a repo to your GOPATH and skip the building and installation phase.

# Tools

## `go vet`

The `vet` command will check your code for common errors. Let's look at the types of errors `vet` can catch:

- Bad parameters in Printf-style function calls
- Method signature errors for common method definitions
- Bad struct tags
- Unkeyed composite literals

```go
package main

import "fmt"

func main() {
    // Printf calls whose arguments do not align with the format string.
    fmt.Printf("The quick brown fox jumped over lazy dogs", 3.14)
}
```
```
$ go vet main.go
main.go:6: no formatting directive in Printf call
```

## `go list`

It lists the packages named by the import paths, one per line.

## `go env`

It prints Go environment information.

```
$ go env
GO111MODULE=""
GOARCH="amd64"
GOBIN=""
...
```

If there are specific values that you're interested in, you can pass them as arguments to go env.

```
$ go env GOPATH GOOS GOARCH
/home/nana/go
linux
amd64
```

## `go fmt`

Gofmt is a tool that automatically formats Go source code.

To format your code, you can use the gofmt tool directly:

```bash
gofmt -w yourcode.go
```
Or you can use the “go fmt” command:

```bash
go fmt path/to/your/package
```

It also supports rewrite rules that you can use to help refactor your code.

```go
var foo int

func bar() {
    foo = 1
	fmt.Println("foo")
}
```
```
$ gofmt -d -w -r 'foo -> Foo' .
-var foo int
+var Foo int

 func bar() {
-	foo = 1
+	Foo = 1
 	fmt.Println("foo")
 }
```