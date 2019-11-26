# 回答例

課題3

```Dockerfile
FROM ubuntu:18.04

RUN apt-get update && apt-get install -y tree iputils-ping && apt-get clean

ENV TARGETPATH=/test/

COPY . ${TARGETPATH}

CMD ["sh", "-c", "echo TARGETPATH=${TARGETPATH}; ping -c 4 8.8.8.8; tree ${TARGETPATH}"]
```
