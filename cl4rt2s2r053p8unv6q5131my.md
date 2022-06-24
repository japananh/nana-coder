## Refactoring - Part 3

I had a requirement to update an API today. The modified function had 8 parameters that break the [SonarQube](https://www.sonarqube.org/) rule, so I decided to refactor it. There are some solutions to the problem. They can be applied similarly in other languages. In this post, I will demonstrate some of them in the Go language.

* For someone don't know SonarQube: **SonarQube** is an open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, and code smells in 17 programming languages.

# Use Parameter Object

The most common way I often do this is to replace parameters with a data structure such as `struct` in Go or an `object` or a `class` in other languages such as JavaScript. 

The benefits of it are more readable code and reusable code. Instead of parameters, you see a single object which can be reused somewhere.

```go
// Before

package service

import (
    "context"
	"database/sql"
)

// There are 8 parameters here

func CreateUser(ctx context.Context, db *sql.DB, email, password, firstName, lastName, addr string, status int) error {
	// do something
	return nil
}
```

```go
// After

package service

import (
	"context"
	"database/sql"
)

type CreateUserRequestParams struct {
	Status                                int
	Email, Password, FirstName, LastName, Addr string
}

// The code is readable thanks to fewer parameters

func CreateUser(ctx context.Context, db *sql.DB, p *CreateUserRequestParams) error { 
	// do something 
	return nil
}
```

The drawback of it is [Data class](https://refactoring.guru/smells/data-class) - the class defines instance variables but lacks relevant methods. Such classes are very likely being manipulated by other classes heavily.

# Use Currying

One popular way in Functional programming is to use the **Currying**.  It's named after the guy who invented the technique, [Haskell Brooks Curry](https://en.wikipedia.org/wiki/Haskell_Curry). People apply this pattern a lot in JavaScript libraries such as [lodash](https://lodash.com/). Currying is a transform that makes `f(a,b,c)` callable as `f(a)(b)(c)`. One of its benefits is to shorten how many arguments a function requires. You can read more about currying [here](https://jessewarden.com/books/real-world-functional-programming/part5/).

```go
// After

package service

import (
	"context"
	"database/sql"
)

func CreateUser(ctx context.Context, db *sql.DB) func(email, password, firstName, lastName, addr string, status int) error {
  return func (email, password, firstName, lastName, addr string, status int) error { 
	// do something 
	return nil
  }
}

// You can call it like below
// service.CreateUser(ctx, db)(email, password, firstName, lastName, addr, status)
```

# Refactor the function with the Extract Method

If your function takes too many parameters, it might do too much work and should be broken into smaller functions, consequently, reducing the arguments’ number. The [Extract Method](https://refactoring.guru/extract-method) technique can be used to achieve this goal. 

# Conclusion

In my case, the Parameter Object is the best choice. I just wanna dive deep into how many ways I can apply to reduce the parameters so that's why I mention other solutions here. Hope that can help you.
