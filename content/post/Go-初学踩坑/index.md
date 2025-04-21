---
title: Go 初学踩坑
draft: false
math: true
date: 2021-12-13 14:53:28
cover: images/featureimages/2.jpg
summary: 数据库系统课程笔记
summary: Go 语言初学踩坑记录
tags: [graduate learning,Go]
categories: [Go]
---

## 初级篇

1. 开大括号不能放在单独的一行
在大多数其他使用大括号的语言中，需要选择放置它们的位置。Go 的方式不同。由于自动分号的注入（没有预读），不需要手动添加分号
失败的例子：

```Go
package main
import "fmt"
func main()  
{ //error, can't have the opening brace on a separate line
    fmt.Println("hello there!")
}
```

编译错误：

```bash
/tmp/sandbox826898458/main.go:6: syntax error: unexpected semicolon or newline before {
```

正确示例：

```Go
package main
import "fmt"
func main() {  
    fmt.Println("works!")
}
```

2. 未使用的变量

如果有未使用的变量，代码将编译失败。当然也有例外。在函数内一定要使用声明的变量，但未使用的全局变量是没问题的。
如果给未使用的变量分配了一个新的值，代码还是会编译失败。需要在某个地方使用这个变量，才能让编译器编译通过。

Fail:

```Go
package main
var gVar int //not an error
func main() {  
    var one int   //error, unused variable
    two := 2      //error, unused variable
    var three int //error, even though it's assigned 3 on the next line
    three = 3     
}
```

Compiles error:

```bash
/tmp/sandbox473116179/main.go:6: one declared and not used
/tmp/sandbox473116179/main.go:7: two declared and not used
/tmp/sandbox473116179/main.go:8: three declared and not used
```

Works:

```Go
package main
import "fmt"
func main() {  
    var one int
    _ = one
    two := 2 
    fmt.Println(two)
    var three int 
    three = 3
    one = three
    var four int
    four = four
}
```

3. 未使用的 import

如果引入一个包，而没有使用其中的任何函数、接口、结构体或者变量的话，代码将会编译失败。
可以使用 goimports 来增加引入或者移除未使用的引用：

```bash
$ go get golang.org/x/tools/cmd/goimports
```

Fails:

```Go
package main
import (
    "fmt"
    "log"
    "time"
)
func main() {
}
```

编译错误

```bash
/tmp/sandbox627475386/main.go:4: imported and not used: "fmt"
/tmp/sandbox627475386/main.go:5: imported and not used: "log"
/tmp/sandbox627475386/main.go:6: imported and not used: "time"
```

Works:

```Go
package main
import (
    _ "fmt"
    "log"
    "time"
)
var _ = log.Println
func main() {
    _ = time.Now
}
```

4. 简式的变量声明仅可以在函数内部使用

Fails:

```Go
package main
myvar := 1 //error
func main() {
}
```

Compile Error:

```bash
/tmp/sandbox265716165/main.go:3: non-declaration statement outside function body
```

Works:

```Go
package main
func main() {
    one := 0
    one, two := 1,2
    one,two = two,one
}
```

5. 偶然的变量隐藏 Accidental Variable Shadowing

Wrong:

```go
package main
import "fmt"
func main() {
    x := 1
    fmt.Println(x)     //prints 1
    {
        fmt.Println(x) //prints 1
        x := 2
        fmt.Println(x) //prints 2
    }
    fmt.Println(x)     //prints 1 (bad if you need 2)
}
```

可以使用 `vet` 命令来发现一些这样的问题。默认情况下， vet不会执行这样的检查，需要设置 -shadow参数：

```bash
go tool vet -shadow your_file.go
```

6. 不使用显式类型，无法使用“nil”来初始化变量

`nil` 标志符用于表示 `interface` 、`func`、 `maps` 、 `slices` 和 `channels` 的“零值”。如果你不指定变量的类型，编译器将无法编译你的代码，因为它猜不出具体的类型。

Fails:

```bash
package main
func main() {
    var x = nil //error
    _ = x
}
```

Compiler Error:

```bash
/tmp/sandbox188239583/main.go:4: use of untyped nil
```

Works:

```bash
package main
func main() {
    var x interface{} = nil
    _ = x
}
```

8. 使用“nil” Slices and Maps

在一个 nil 的 slice 中添加元素是没问题的，但对一个 map 做同样的事将会生成一个运行时的 panic 。
Works:

```bash
package main
func main() {
    var s []int
    s = append(s,1)
}
```

Fails:

```bash
package main
func main() {
    var m map[string]int
    m["one"] = 1 //error
}
```

9. map的容量

可以在 `map` 创建时指定它的容量，但你无法在 `map` 上使用 `cap()` 函数。
Fails:

```go
package main
func main() {
    m := make(map[string]int,99)
    cap(m) //error
}
```

Compile Error:

```bash
/tmp/sandbox326543983/main.go:5: invalid argument m (type map[string]int) for cap
```

10. 字符串不会为nil

Fails:

```go
package main
func main() {
    var x string = nil //error
    if x == nil { //error
        x = "default"
    }
}
```

Compile Errors:

```bash
/tmp/sandbox630560459/main.go:4: cannot use nil as type string in assignment /tmp/sandbox630560459/main.go:6: invalid operation: x == nil (mismatched types string and nil)
```

Works:

```go
package main
func main() {
    var x string //defaults to "" (zero value)
    if x == "" {
        x = "default"
    }
}
```

11. array 函数的参数


C 或 C++ 中，数组就是指针。当你向函数中传递数组时，函数会参照相同的内存区域，这样它们就可以修改原始的数据。

Go 中的数组是数值，因此当向函数中传递数组时，函数会得到原始数组数据的一份复制。如果打算更新数组的数据，这将会是个问题。

```go
package main
import "fmt"
func main() {
    x := [3]int{1,2,3}
    func(arr [3]int) {
        arr[0] = 7
        fmt.Println(arr) //prints [7 2 3]
    }(x)
    fmt.Println(x) //prints [1 2 3] (not ok if you need [7 2 3])
}
```

如果需要更新原始数组的数据，可以使用数组指针类型。

```go
package main
import "fmt"
func main() {
    x := [3]int{1,2,3}
    func(arr *[3]int) {
        (*arr)[0] = 7
        fmt.Println(arr) //prints &[7 2 3]
    }(&x)
    fmt.Println(x) //prints [7 2 3]
}
```

另一个选择是使用 `slice`。即使函数得到了 `slice` 变量的一份拷贝，它依旧会参照原始的数据。即对 `slice` 的修改会修改原数组。

```go
package main
import "fmt"
func main() {
    x := []int{1,2,3}
    func(arr []int) {
        arr[0] = 7
        fmt.Println(arr) //prints [7 2 3]
    }(x)
    fmt.Println(x) //prints [7 2 3]
}
```



