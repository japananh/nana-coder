## Go interview questions and answers

# 1. Explain packages in Go.

Every Go program is made up of packages. The program starts running in package `main`. This program is using packages with import paths such as `fmt`.

```
package main

import (
    `fmt`
)

func main() {
    fmt.Println("Hello word!")
}
```

# 2. What is `GOPATH`?

The `GOPATH` is an environment variable that determines the location of the workspace. It is the only variable that you have to set when developing Go code.

# 3. How to use custom packages?

You have the folder structure and two files as below.

```markdown
src/
    myproject/
        mylib/
            mylib.go
            ...
        main.go
```

**mylib.go**

```go
package mylib

type SomeType struct {
    ...
}
```

**main.go**

```go
package main

import (
    "mylib"
)

func main() {
    ...
}
```

If you want to use `mylib` in `main.go`, you could go like this:

1. Place the directory with library files under the directory of your project.

2. In the rest of your project, refer to the library using its path relative to the root of your workspace containing the project.

Now, in the top-level main.go, you could import "myproject/mylib" and it would work.

**main.go**

```go
package main

import (
    "myproject/mylib" // WORK
)

func main() {
    ...
}
```

# 4. Explain what is goroutine and how to stop goroutine?

A goroutine is a function that is capable of running concurrently with other functions.

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 1; i <= 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	go say("world")
	say("hello")
}
```

```bash
$ go run main.go
world
hello
hello
world
world
hello
hello
world
world
hello
```

To stop goroutine, you pass the goroutine a signal channel, that signal channel is used to push a value into when you want the goroutine to stop. The goroutine polls that channel regularly as soon as it detects a signal, it quits.

```go
package main

import (
	"fmt"
	"time"
)

func say(s string, quit chan bool) {
	defer close(quit)
	
    for i := 1; i <= 5; i++ {
		fmt.Println(s, i)
		if i == 3 {
			quit <- true
		}
	}

	for {
		select {
		case <-quit:
			return
		default:
			// Do other stuff
		}
	}
}

func main() {
	// a channel to signal that it's stopped
	quit := make(chan bool)
	
    go say("Hello", quit)
    
    // Sleep the program to avoid go routine is forced to exit because `main()` exits
	time.Sleep(1000 * time.Millisecond)
}
```

```bash
$ go run main.go
Hello 1
Hello 2
Hello 3
```