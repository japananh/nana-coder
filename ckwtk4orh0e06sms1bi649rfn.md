## Refactoring - Part 1

This post was inspired by the book **Refactoring**, which was written by Martin Fowler and Kent Beck.

# Extract Function

The term "function" can be interchanged with "method" in an object-oriented language. The idea behinds it is that you look at a fragment of code, understand what it is doing, then extract it into its own function named after its purpose. With this principle, you can develop a habit of writing very small functions.

Take a look at the code below. We have a struct `Rectangle` and a method `PrintInfo()` to log the name and area of it. 

```go
package main

import "fmt"

type Rectangle struct {
	name   string
	width  int
	height int
}

func (s *Rectangle) PrintInfo() {
	fmt.Println("name:" + s.name)
	area := s.height * s.width
	fmt.Println("Area:" + fmt.Sprintf("%d", area))
}

func main() {
	rect := Rectangle{"Rect 1", 10, 5}
	rect.PrintInfo()
	// Name:Rect 1
	// Area:50
}
```

Applying the extract function rule here, we can extract calculation part `area := s.height * s.width` into a new method `GetArea`. Imagine you have a ton of calculations in `PrintInfo()`, the extract function will improve readability in your code.
 
```go
package main

import "fmt"

type Rectangle struct {
	name   string
	width  int
	height int
}

func (s *Rectangle) PrintInfo() {
	fmt.Println("name:" + s.name)
	fmt.Println("Area:" + fmt.Sprintf("%d", s.GetArea()))
}

func (s *Rectangle) GetArea() int {
	return s.height * s.width
}

func main() {
	rect := Rectangle{"Rect 1", 10, 5}

	rect.PrintInfo()

	// Name:Rect 1
	// Area:50
}
```