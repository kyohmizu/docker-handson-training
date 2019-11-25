# 回答例

課題1

```bash
# ①
$ sudo docker container run -itd --rm --name hw-demo -p 9999:80 \
dockercloud/hello-world

# ②
$ sudo docker container ls | grep hw-demo

# ③ http://[ホストのIPアドレス]

# ④
$ sudo docker container stop hw-demo
```

課題2

```go:main.go
package main

import "fmt"

func main() {
  fmt.Println("Golang Sample")
}
```

```bash
$ ls
main.go

$ sudo docker container run -it -v $(pwd):/go/src/sample-go \
golang:alpine go build -o /go/src/sample-go sample-go

$ ls
main.go  sample-go

$ ./sample-go
Golang Sample
```
