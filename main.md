class: center, middle, inverse
## Dockerハンズオン

---
exclude: true
### whoami

.left-small[
    ![image](https://pbs.twimg.com/profile_images/994762110792953856/EheEvqBY_400x400.jpg)
]

.right-large[
- Kyohei Mizumoto(@kyohmizu)

- C# Software Engineer

- Interests
    - Docker/Kubernetes
    - Go
    - Security
]

---
### 対象者

- Docker触ったことない人
- コンテナって…？な人

---
### 今日のゴール

- コンテナについて何となく理解する
- Dockerfileを書けるようになる
- Dockerコマンドを使えるようになる

---
### アウトライン

1. 事前準備
1. コンテナの基本
1. Docker概要
1. ハンズオン
   1. Dockerの基本操作
   2. Dockerfile
   3. 管理ツール

---
class: center, middle, blue
## 事前準備

---
### 実行環境

.zoom2[
Docker CEをインストールしたマシンが必要です

```bash
$ sudo docker version
```

サポートOS  
<u><https://docs.docker.com/install/#supported-platforms/></u>


Azure仮想マシン(Ubuntu)でDockerを始めるには、以下を参考にしてください  
<u><https://qiita.com/kyohmizu/items/7bc0bec96cc4664473eb></u>
]

---
### ツール等

.tmp[
- git
  - サンプルコードのインストールに便利  
    <u><https://git-scm.com/></u>
]
.tmp[
- ssh  
  - 仮想マシンに接続する場合は必須
  - Git Bashも可
]
- Docker Hubのアカウント作成  
  <u><https://hub.docker.com/></u>

---
class: center, middle, blue
## コンテナの基本

---
### コンテナ？

- 仮想化技術の一つ(コンテナ型仮想化)  
  ⇔ 仮想マシン(VM)
- 1つのホスト上に複数の分離空間(=コンテナ)を作成
  - ホストのプロセスとして動作
  - それぞれのコンテナでは異なるOSを実行可能
- ホストOSのカーネルを共有
- Dockerコンテナが主流

---
.left-half[
  仮想マシン
  <img src="https://docs.microsoft.com/ja-jp/dotnet/architecture/microservices/container-docker-introduction/media/image3.png" width=84%>
]

.right-half[
  コンテナ
  <img src="https://docs.microsoft.com/ja-jp/dotnet/architecture/microservices/container-docker-introduction/media/image4.png" width=84%>
]

.zoom0-r[
  <u><https://docs.microsoft.com/ja-jp/dotnet/architecture/microservices/container-docker-introduction/docker-defined></u>
]

---
### コンテナの特徴

仮想マシンとの相違

- 軽量(オーバーヘッドが少ない)
- 起動が高速
- 分離レベルはあまり高くない
  - セキュリティリスクに注意  
    → Rootlessコンテナの利用  
    → gVisorによるサンドボックス化

---
### コンテナを支える技術

.half[
- namespace
  - プロセスID、ユーザ、ファイルシステム等を分離
  - コンテナからホストのプロセス、ユーザは見えない
]

- cgroups
  - CPU、メモリ等のマシンリソースを分離
  - リソースの使用量を制限

---
class: center, middle, blue
## Docker概要

---
### Docker？

.color-dark[
>Dockerは、インフラ関係やDevOps界隈で注目されている技術の一つで、Docker社が開発している、コンテナ型の仮想環境を作成、配布、実行するためのプラットフォームです。
]

.zoom0-r[
  <u><https://knowledge.sakura.ad.jp/13265/></u>
]

---
### Dockerの利点

- コードで管理(Infrastructure as Code)
  - 誰でも環境を再現できる
  - Single Source of Truth
- 可搬性が高い
  - 作成した環境の配布が容易
- 起動が高速
  - 開発効率を高める
  - CI/CDと相性が良い

---
### 用語

.zoom2[
- Dockerエンジン
  - クライアント・サーバー型プログラム
- Dockerイメージ
  - コンテナの基となるファイル
  - Dockerfileから作成される
- Dockerコンテナ
  - コンテナの本体
  - Dockerイメージから作成される
]

---
### 用語

.zoom2[
- Dockerレジストリ
  - Dockerイメージを保存するサービス
  - Docker Hubやプライベートレジストリ
- Dockerリポジトリ
  - タグが異なる同じ名前のDockerイメージ群
]

---
### Dockerエンジン構成

.half-3[
<center><img src="http://docs.docker.jp/v1.12/_images/engine-components-flow.png" width=82%></center>
]

.zoom0-r[
  <u><http://docs.docker.jp/v1.12/engine/understanding-docker.html></u>
]

---
### アーキテクチャ

.half-3[
<center><img src="http://docs.docker.jp/v1.12/_images/architecture.png" width=100%></center>
]

.zoom0-r[
  <u><http://docs.docker.jp/v1.12/engine/understanding-docker.html></u>
]

---
class: center, middle, blue
## Dockerの基本操作

---
### CLIツール

```bash
$ sudo docker [サブコマンド] [オプション] [ターゲット]
```

※管理者権限が必要なため sudo をつける

サブコマンド

- container
- image
- version   
  etc...

---
### イメージをレジストリから取得

```bash
$ sudo docker image pull hello-world
Using default tag: latest

(途中略)

Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

- デフォルトのレジストリはDocker Hub
- タグを指定する場合は イメージ名:タグ名

---
### イメージの一覧を取得

```bash
$ sudo docker image ls
REPOSITORY                                        TAG   
          IMAGE ID            CREATED             SIZE
hello-world                                       latest
          fce289e99eb9        10 months ago       1.84kB
```

- リポジトリとタグで一意に表される

---
### コンテナを作成＆実行

.zoom2[
```bash
$ sudo docker container run hello-world

Hello from Docker!
This message shows that your installation appears to 
be working correctly.

(以下略)
```

- 以下2つのコマンドと同じ
  - docker container create
  - docker container start
- 実行結果が出力され、コンテナは終了する
]

---
### コンテナを作成＆実行

.zoom2[
```bash
$ sudo docker container run -it ubuntu:18.04
Unable to find image 'ubuntu:18.04' locally
18.04: Pulling from library/ubuntu

(途中略)

Status: Downloaded newer image for ubuntu:18.04
root@7bc89a31691b:/
```

- ローカルにイメージがない場合、最初にイメージを取得
- -it：インタラクティブなコンテナを作成
  - 実行プロセスは /bin/bash
- exit でコンテナを終了
]

---
### コンテナを作成＆実行

.zoom2[
```bash
$ sudo docker container run -it -d -p 8080:80 \
dockercloud/hello-world
d11972fbee5201b13a03cf296f1ee0e58a5371a178083b69c913d6177365...
```

- -d：バックグラウンドでコンテナを実行
  - コンテナIDが出力される
- -p：コンテナ内とホストのポートフォワード
  - ブラウザでアクセスしてみる  

```txt
  http://[ホストのIPアドレス]:8080
```
]

---
### コンテナを作成＆実行

<center><img src="hello-world.png" width=100%></center>

---
### コンテナの一覧を取得

.zoom1[
```bash
$ sudo docker container ls
CONTAINER ID     IMAGE                     COMMAND                CREATED    
STATUS              PORTS                  NAMES
d11972fbee52     dockercloud/hello-world   "/bin/sh -c /run.sh"   22 minutesago
Up 29 minutes       0.0.0.0:8080->80/tcp   heuristic_hellman

$ sudo docker container ls -a
CONTAINER ID     IMAGE                     COMMAND                CREATED   
STATUS                      PORTS                  NAMES
866926f90fec     hello-world               "/hello"               3 minutes ago
Exited (0) 50 minutes ago                          nice_leakey
d11972fbee52     dockercloud/hello-world   "/bin/sh -c /run.sh"   23 minutesago
Up 23 minutes               0.0.0.0:8080->80/tcp   heuristic_hellman
7bc89a31691b     ubuntu:18.04              "/bin/bash"            42 minutesago
Exited (0) 32 minutes ago                          pedantic_hopper
```
]

.zoom2[
- オプションなしの場合は、実行中のコンテナのみ取得
- -a をつけることで全てのコンテナを取得できる
]

---
### コンテナを削除

.zoom2[
```bash
$ sudo docker container rm 866926f90fec
866926f90fec

// hello-worldイメージのコンテナをすべて削除
$ sudo docker container ls -a | grep hello-world | \
cut -d ' ' -f1 | sudo xargs docker container rm
```

- コンテナIDまたはコンテナ名を指定して削除
- 以下のコマンドで確認

```bash
$ sudo docker container ls -a
```
]

---
### イメージを削除

.zoom2[
```bash
$ sudo docker image rm hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:c3b4ada4687bbaa170745b3e4dd8ac3...
Deleted: sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474...
Deleted: sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233...
```

- ローカルに取得したイメージを削除
- コンテナが残っている場合は削除できない
  - -f：コンテナごと削除可能
- 以下のコマンドで確認

```bash
$ sudo docker image ls
```
]

---
### コンテナの詳細を表示

.zoom2[
```bash
$ sudo docker container inspect d11972fbee52
[
    {
        "Id": "d11972fbee5201b13a03cf296f1ee0e58a5371a1780...",
        "Created": "2019-10-29T06:27:33.043549168Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "/run.sh"
        ],

(途中略)

    }
]
```
]

---
### コンテナを停止

.zoom2[
```bash
$ sudo docker container stop d11972fbee52
d11972fbee52
```

- コンテナIDまたはコンテナ名を指定して停止
- 以下のコマンドで再実行

```bash
$ sudo docker container start d11972fbee52
d11972fbee52
```
]

---
### コンテナにアタッチ

.zoom2[
```bash
// コンテナをバックグラウンドで実行
$ sudo docker container run -itd --name ubuntu ubuntu:18.04
18d9ca0f84c823960f81e769660b267e45c9176cbd1e3e28db915dfd6...

$ sudo docker container attach ubuntu
root@18d9ca0f84c8:/#
```

- --name：コンテナ名を設定
- Ctrl-p + Ctrl-q：コンテナからデタッチ
- Ctrl-c：コンテナの終了
- exit でもコンテナが終了する
]
---
### コンテナ内でコマンド実行

.zoom2[
```bash
// コンテナを起動
$ sudo docker container start ubuntu
ubuntu

$ sudo docker container exec -it ubuntu /bin/bash
root@18d9ca0f84c8:/#
```

- コンテナ内に存在するコマンドのみ
- exit で終了するのは実行プロセス
  - コンテナは終了しない
]

---
### その他

- レジストリからイメージを検索

```bash
$ sudo docker search [検索ワード]
```

- Dockerに関するシステム情報を表示

```bash
$ sudo docker info
```

---
class: center, middle, blue
## Dockerfile

---
### Dockerfile？

- Dockerイメージの基となるテキストファイル
- イメージの作成に必要な全情報を含む
  - ベースイメージ
  - コピーするファイル
  - 実行ユーザー
  - ポート
  - 実行コマンド  
    etc...

---
### Dockerfileリファレンス

.tmp[
- FROM
  - ベースイメージを指定
]
.tmp[
- COPY
  - ホストからコンテナ内にファイルをコピー
]
.tmp[
- ADD
  - ホストからコンテナ内にコピーして展開
]
.tmp[
- RUN
  - イメージビルド時に、コンテナ内でコマンドを実行
]

---
### Dockerfileリファレンス

.tmp[
- CMD
  - コンテナ内でコマンドを実行
  - docker container run時に上書き可
]
.tmp[
- ENTRYPOINT
  - コンテナ内でコマンドを実行
]
.tmp[
- WORKDIR
  - 作業ディレクトリを変更
]

---
### Dockerfileリファレンス

.tmp[
- ENV
  - コンテナ内で環境変数を設定
]
.tmp[
- USER
  - ユーザーを切り替え
]
.tmp[
- EXPOSE
  - 指定したポートでリッスン
]
.tmp[
- VOLUME
  - ボリュームとして扱うディレクトリを指定
]

---
### Dockerfileの作成①(centos)

.zoom0[
  <u><https://github.com/kyohmizu/docker-handson-sample/blob/master/sample1/Dockerfile></u>
]

.zoom2[
```bash
# ディレクトリを作成＆移動
$ mkdir -p ~/docker/sample1; cd ~/docker/sample1

# Dockerfileを作成(出力結果を参考に)
$ cat Dockerfile
FROM centos:7.5.1804
COPY test /sample/
RUN echo "sample docker container created" > /sample/doc

# コンテナにコピーするファイルを作成
$ echo "test" > test
$ ls
Dockerfile  test
```
]

---
### Dockerfileの作成①(centos)

.zoom2[
```bash
# イメージを作成
# -f：Dockerfileを指定
# -t：タグを設定
$ sudo docker image build -f ./Dockerfile -t test .

# コンテナを起動
# --rm：コンテナの停止時にコンテナを削除
$ sudo docker container run -it --rm test
# コンテナ内にアクセスした状態になる(以下はコンテナ内)

# ファイルを確認
$ cat /sample/doc
$ cat /sample/test
```
]

---
### Dockerfileの作成②(httpd)

.zoom0[
  <u><https://github.com/kyohmizu/docker-handson-sample/blob/master/sample2/Dockerfile></u>
]

.zoom2[
```bash
$ mkdir -p ~/docker/sample2; cd ~/docker/sample2

# Dockerfileを作成(出力結果を参考に)
$ cat Dockerfile
FROM centos:7.5.1804
RUN yum install -y httpd iproute && yum clean all
COPY index.html /var/www/html/
RUN systemctl enable httpd
CMD ["/sbin/init"]

# ブラウザで表示するファイルを作成
$ vi index.html
$ ls
Dockerfile  index.html
```
]

---
### Dockerfileの作成②(httpd)

.zoom2[
```bash
# イメージを作成
$ sudo docker image build -f ./Dockerfile -t web-httpd:v1 .

# コンテナを起動
# --tmpfs：オンメモリのディレクトリを作成
# --mount：ホストのディレクトリにバインド
# --stop-signal：停止時のシグナルを設定
$ sudo docker container run -itd --tmpfs /tmp --tmpfs /run \
--mount type=bind,src=/sys/fs/cgroup,dst=/sys/fs/cgroup \
--stop-signal SIGRTMIN+3 --name web01 -p 8080:80 \
web-httpd:v1
f1e764371f48e053fec5ba81ba52fce2c35eb2d7bbb5fdc995e4138e1...
```
]

---
### Dockerfileの作成②(httpd)

.zoom2[
```bash
# コンテナを確認
$ sudo docker container ls

# Webサービスにアクセス
$ curl http://localhost:8080
```

ブラウザでアクセス

```txt
  http://[ホストのIPアドレス]:8080
```
]

---
### Dockerfileの作成③(postgreSQL)

.zoom0[
  <u><https://github.com/kyohmizu/docker-handson-sample/blob/master/sample3/Dockerfile></u>
]

.zoom2[
```bash
$ mkdir -p ~/docker/sample3; cd ~/docker/sample3

# Dockerfileを作成(上部リンクを参照)
$ vi Dockerfile
$ ls
Dockerfile

# イメージを作成
$ sudo docker image build -f ./Dockerfile -t pg-test .

# コンテナを起動
# --P：コンテナのポートをホストに公開(公開ポートはランダム)
$ sudo docker container run -d -P --rm --name pg-test pg-test
```
]

---
### Dockerfileの作成③(postgreSQL)

.zoom2[
```bash
# ポートを確認
$ sudo docker container ls pg-test
CONTAINER ID   IMAGE       COMMAND                  CREATED    
        STATUS              PORTS                     NAMES
eb21a31e9755   pg-test     "/usr/lib/postgresql…"   43 seconds 
ago     Up 38 seconds       0.0.0.0:32773->5432/tcp   pg-test

# DBに接続
# psql コマンドを使用するには、postgresql のインストールが必要
# sudo apt install postgresql
$ psql -h localhost -p 32773 -d docker -U docker --password
```
]

---
### Dockerfileの作成③(postgreSQL)

.zoom2[
```bash
# DBに接続している状態
# テーブルを作成
$ CREATE TABLE cities (
    name            varchar(80),
    location        point
);
CREATE TABLE

# データを挿入
$ INSERT INTO cities VALUES 
('San Francisco', '(-194.0, 53.0)');
INSERT 0 1

# データを確認
$ SELECT * FROM cities;
```
]

---
### Dockerfileの作成④(Web API)

.zoom0[
  <u><https://github.com/kyohmizu/docker-handson-sample/blob/master/sample4/></u>
]

.zoom2[
```bash
# 上記リンクのサンプルプログラムを使用します
$ cd go-api; ls
go.mod  go.sum  main.go

# Dockerfileを作成
$ vi Dockerfile

# イメージを作成
$ sudo docker image build -f ./Dockerfile -t go-api .

# コンテナを起動
$ sudo docker container run -itd --rm \
-p 9999:9999 --name go-api go-api
```
]

---
class: center, middle, blue
## 管理ツール

---
### Docker Compose

---
### 参考

.zoom1[
公式ドキュメント  
<u><https://docs.docker.com/></u>

Dockerドキュメント日本語化プロジェクト  
<u><http://docs.docker.jp/v1.12/index.html></u>

Dockerコマンドリファレンス  
<u><https://docs.docker.com/engine/reference/commandline/docker/></u>

Docker pull commit push Dockerfile WordPress環境をつくる
<u><https://qiita.com/cyberblack28/items/7ed6fbc75503bf9418b3></u>
]

---
class: center, middle, blue
# おわり
