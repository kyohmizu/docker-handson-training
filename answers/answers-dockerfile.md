# 回答例

課題3

```Dockerfile
# apt を使用するので、ベースイメージを ubuntu に変更
FROM ubuntu:18.04

# インストール前にリポジトリの更新が必要 (完了後はcleanを実行)
# tree パッケージのインストールが必要
RUN apt-get update && apt-get install -y tree iputils-ping && apt-get clean

# 変数に初期値を設定
ENV TARGETPATH=/test/

COPY . ${TARGETPATH}

CMD ["sh", "-c", "echo TARGETPATH=${TARGETPATH}; ping -c 4 8.8.8.8; tree ${TARGETPATH}"]
```
