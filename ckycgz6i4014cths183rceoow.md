## Hàm int trong Go

Bài này ghi lại những thứ tôi hiểu về hàm init trong Go.

Khi tạo ứng dụng với ngôn ngữ Go, bạn cần khởi tạo biến cùng giá trị của chúng. Một trong những cách để làm điều này là khởi tạo chúng ở đầu hàm `main()` hoặc khởi tạo chúng như biến global. Hàm `init()` cho phép bạn chạy code trước hàm `main()` và sau khi biến global được khởi tạo.

# Concepts

Hàm `init()` sẽ được gọi trước hàm `main()` theo thứ tự như hình dưới.

![1_IXfNKuWDM_g1xijwpd254w.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642044098355/G8Kuy4BXi.png)

# Requirements

Để chạy được phần code demo, bạn cần cài [Go](https://go.dev/doc/install) và [nodemon](https://www.npmjs.com/package/nodemon). Bạn có thể chọn bất kỳ editor nào. Tôi dùng [Visual Studio Code](https://code.visualstudio.com/download).

# Demo

- Hàm `init()` chỉ chạy một lần.
- Hàm `init()` sẽ chạy sau khi biến global của package được khởi tạo và trước hàm `main()`.

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("run main")
}

func init() {
	fmt.Println("init package main")
	fmt.Println("x =", x)
}

var x int = 1 // biến global

```
```bash
nodemon --exec go run main.go --signal SIGTERM
```
```bash
init package main
x = 1
run main
```
- Hàm `init()` bên trong package sẽ chạy khi import package bất kể bạn có sử dụng package này hay không.

```go
package main

import (
	"fmt"
	_ "learn/internal/collections"
)

func main() {
	fmt.Println("run main")
}
```
```go
package collections

import "fmt"

func init() {
	fmt.Println("init package collections")
}

func main() {
	fmt.Println("run main in collections")
}

```
```bash
init package collections
run main
```
- Nếu có nhiều hàm `init()` trong một file thì thứ tự chạy `init()` sẽ từ trên xuống dưới.

```go
package main

import (
	"fmt"
)

func init() {
	fmt.Println("run init 1")
}

func init() {
	fmt.Println("run init 2")
}

func main() {
	fmt.Println("run main")
}
```
```bash
run init 1
run init 2
run main
```
- Nếu có nhiều hàm `init()` giữa các file trong một package, thứ tự chạy theo *tên file tăng dần theo bảng chữ cái tiếng anh*.

```go
package main

import (
	"fmt"
	_ "learn/pkg/collections"
	_ "learn/pkg/rabbits"
)

func init() {
	fmt.Println("init package main")
}

func main() {
	fmt.Println("run main")
}
```
```go
package collections

import "fmt"

func init() {
	fmt.Println("init package collections")
}
```
```go
package rabbits

import "fmt"

func init() {
	fmt.Println("init package rabbits")
}
```
```bash
init package collections
init package rabbits
init package main
```

# Nhược điểm

Sau đây là một vài nhược điểm của việc dùng hàm `init()`:

- Làm chậm thời gian khởi động của ứng dụng.
- Hàm `init()` sẽ được gọi ngay khi bạn import package bất kể có cần hay không.
- Nếu hàm `init()` sửa giá trị của biến global, nó sẽ không được gọi khi chạy test.

# Tài liệu tham khảo

- https://david-yappeter.medium.com/init-in-go-programming-31e2c2bc2371
- https://go.dev/doc/effective_go#init