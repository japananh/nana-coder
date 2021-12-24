## Generics in Go

Go team released Go 1.18 Beta 1 in December 2021, introducing support for generic programming using type parameters, a much-anticipated feature. 

Note: If you prefer, you can use the [Go playground in “Go dev branch” mode](https://go.dev/play/?v=gotip) to try new features with generics.

# What is new in generics?

## Type parameters for functions and types

Type parameter lists look like ordinary parameter lists with square brackets. It is customary to start type parameters with upper-case letters to emphasize that they are types.

```go
[P, Q constraint1, R constraint2]
```

Let's take a look at the function `min` below. 

```go
func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

m := min(2, 3)
fmt.Println(m) // 2
```

If we want this function to work with other number types such as `float64`, we need to declare a new function to handle the new type. To solve that problem, we use generics. The type parameter `T`, declared in a type parameter list, takes the place of `int`.

```go
func min[T constraints.Ordered](x, y T) T {
	if x < y {
		return x
	}
	return y
}

m := min[int](2, 3)
n := min[float64](2, 3)
fmt.Println(m, n) // 2 2
```

## Type sets defined by interfaces

Type parameter lists also have a type for each type parameter. This type defines a set of types. It is called the **type constraint**. In Go, type constraints must be interfaces. 

![Screen Shot 2021-12-24 at 15.12.32.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640333624502/ggVEFQOj9.png)

Let's take a look at the example below.

```go
func min[T constraints.Ordered](x, y T) T
```

`constraints.Ordered` is a type constraint. `Ordered` defines the set of all integer, floating-point, and string types.

`~T` means the set of all types with the underlying type `T`.

Check this [discussion](https://github.com/golang/go/discussions/47319) for more information.

```go
// Package constraints define a set of useful constraints to be used with type parameters.
package constraints

type Ordered interface {
    int | float | ~string
}
```

## Type inference

When declaring a variable without specifying an explicit type (either by using the `:=` syntax or `var =` expression syntax), the variable's type is inferred from the value on the right-hand side.

```go
var i int
j := i // j is an int
```

You can call the `min` function without type inference. Sometimes, passing type arguments lead to more verbose code.

```go
func min[T constraints.Ordered](x, y T) T

var a float64 = 1.1
var b float64 = 2.2
var m float64 = min(a, b) // not need to pass any type of argument here.
fmt.Println(m) // 1.1
```

# References

- https://go.dev/blog/go1.18beta1
- https://www.youtube.com/watch?v=35eIxI_n5ZM
