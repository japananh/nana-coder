## Cool tricks with goroutine

# Using `runtime.Gosched()` to force schedule Goroutines
    
A goroutine can run and occupy a thread for a long time. This should be avoided by using `runtime.Gosched()` to force schedule Goroutines to switch context.
    
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		for i := 1; i <= 50; i++ {
			fmt.Println("I am Goroutine 1")
		}
	}()

	go func() {
		for i := 1; i <= 50; i++ {
			fmt.Println("I am Goroutine 2")
		}
	}()

	time.Sleep(time.Second)
}

// The result will look like below, 
// one goroutine held the thread for so long.
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 2
// I am Goroutine 2
// I am Goroutine 2
// I am Goroutine 2
// I am Goroutine 2
// I am Goroutine 2
// I am Goroutine 1
// ...
```
    
```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	go func() {
		for i := 1; i <= 50; i++ {
			fmt.Println("I am Goroutine 1")
			runtime.Gosched()
		}
	}()

	go func() {
		for i := 1; i <= 50; i++ {
			fmt.Println("I am Goroutine 2")
			runtime.Gosched()
		}
	}()

	time.Sleep(time.Second)
}

// The result will look better like below
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 2
// I am Goroutine 2
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 1
// I am Goroutine 2
// I am Goroutine 2
// ...
```