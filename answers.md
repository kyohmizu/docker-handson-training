# 回答例

課題1

```bash
$ sudo docker container run -itd --rm --name hw-demo -p 9999:80 \
dockercloud/hello-world
```

課題2

```bash
$ ls
main.go

$ sudo docker container run -it -v $(pwd):/go/src/sample-go \
golang:alpine go build -o /go/src/sample-go sample-go

$ ls
main.go  sample-go
```
