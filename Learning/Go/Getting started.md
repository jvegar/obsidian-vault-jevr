#go #getting_started

- Init go module
```sh
$ go mod init example/hello
go: creating new go.mod: module example/hello
```

- hello.go
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

- Run code:
```sh
$ go run .
Hello, World!
```

- Adding external package
```go
package main

import "fmt"

import "rsc.io/quote"

func main() {
    fmt.Println(quote.Go())
}
```
- Add new module requirements
```sh
$ go mod tidy
go: finding module for package rsc.io/quote
go: found rsc.io/quote in rsc.io/quote v1.5.2
```
