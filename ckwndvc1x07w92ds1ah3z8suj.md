## Packages in Go - Part 2


# Imports

The `import` statement tells the compiler where to look on disk to find the package you want to import.

```go
import "fmt"
```

To import more than one package, you need to wrap the import statements in an import block.

```go
import (
    "fmt"
    "strings"
)
```

Packages are found on disk based on their relative path to the directories referenced by `GOPATH`. If the compiler cannot find the package you've referenced in your `GOPATH`, you'll get an error when trying to run or build your program.

 ## Remote imports

The Go tooling has built-in support for fetching source code from distributed version control systems (DVCS) such as sharing sites likes Github.

For example:

```go
import "github.com/dgrijalva/jwt-go" 
```

When an imported path contains a URL, the Go tooling can be used to fetch the package from the DVCS and place the code inside the `GOPATH` at the location that matches the URL. 

## Named imports

Packages with the same name can be imported by using *named imports*.

```go
package main

import (
    "fmt"
    myfmt "mylib/fmt"
)

func main() {
    fmt.Println("Standard library")
    myfmt.Println("My custom library")
}
```

The Go compiler will fail the build and output an error whenever you import a package that you don't use. 

## Blank identifier

Go doesn't allow any unused variable. Any unused variable can be replaced with the `_` (underscore character), known as the *blank identifier*. A blank import of a package is used when the imported package is not being used in the current program, but we need the init function in the Go source files belonging to that package can be called and initialization of variables in that package can be done properly.

As an example mysql package is used as blank import for its side-effect of registering the mysql driver as a database driver in the init()  function of mysql package, without importing any other functions.

```go
_ "github.com/go-sql-driver/mysql"
```

# Init

Each package has the ability to provide as many `init()` functions. They are automatically executed before the `main()` function in the man package is called. 

`init()` function is similar to the `main()` function. It doesn't take any argument nor return anything. Go can support multiple `init()` functions within one file. They are called in their respective order of declaration within the file.

```go
package main

import "fmt"

func init() {
    fmt.Println("init 1")
}

func init() {
    fmt.Println("init 2")
}

func main() {
    fmt.Println("main")
}
```
```bash
$ go run main.go
init 1
init 2
main
```

The `init()` functions are great for setting up packages, initializing variables, or performing any other bootstrapping you may need prior to the program running.

An example of this is database drivers. They register themselves with the SQL package when their init function is executed at startup because the SQL package can’t know about the drivers that exist when it’s compiled. Let’s look at an example of what an init function might do.

```go
package postgres

import (
    "database/sql"
)

func init() {
    sql.Register("postgres", new(PostgresDriver))
}
```
This code lives inside your pretend database driver for the PostgreSQL database. When a program imports this package, the init function will be called, causing the database driver to be registered with Go’s sql package as an available driver.

Now we can tell the `sql.Open` method to use this driver.

```go
package main

import (
    "database/sql"
    _ "https://github.com/goinaction/code/chapter3/dbdriver/postgres"
)

func main() {
    sql.Open("postgres", "mydb")
}
```

# References

- [Go in action](https://www.goodreads.com/en/book/show/22727352-go-in-action)

