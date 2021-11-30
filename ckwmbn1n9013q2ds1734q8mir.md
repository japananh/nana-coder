## Packages in Go - Part 1

# Packages

All Go programs are organized into groups of files called **packages**, so that the code has the ability to be included in other projects as smaller reusable pieces.

![go-package-illustration.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638289452215/QvjsJiOVr.jpeg)

Let’s look at the packages that make up Go’s `http` functionality in the standard library which contains a series of related files with the `.go` extension.

```
net/http/
    cgi/
    cookiejar/
        testdata/
    fcgi/
    httptest/
    httputil/
    pprof/
    testdata/
```

Every Go source file belongs to a package. The package declaration must be the first line of code in your Go source file. All the functions, types, and variables defined in your Go source file become part of the declared package.

```go
package <packagename>
```

## Package-naming convention

The convention for naming your package is to use the name of the directory containing it. 

When naming your packages and their directories, you should use **short, concise, lowercase names**, with **no** `under_scores` or `mixedCaps`. They are often simple nouns, such as:

- `time` (provides functionality for measuring and displaying time)
- `list` (implements a doubly-linked list)
- `http` (provides HTTP client and server implementations)

Package names may be **abbreviated** when the abbreviation is familiar to the programmer. Widely-used packages often have compressed names:

- `strconv` (string conversion)
- `syscall` (system call)
- `fmt` (formatted I/O)

Keep in mind that a unique name is not required because you import the package using its full path.

## The `main` package and `main()` function

Go programs start running in the `main` package. It is a special package that is intended to be compiled into a binary executable. 

The main() function is a special function that is the entry point of an executable program. Let’s see an example of an executable program in Go.

```go
// Package declaration
package main

// Importing packages
import (
	"fmt"
)

func main() {
     fmt.Println("Hello, world!")
}
```

```bash
go run main.go
```

```bash
# Output
Hello, world!
```

