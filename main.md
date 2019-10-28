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
   1. Dockerコマンド
   2. Dockerfile
   3. 管理ツール

---
class: center, middle, blue
## 事前準備

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
## Dockerコマンド

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
### docker image pull

イメージをレジストリから取得  
※デフォルトのレジストリはDocker Hub

```bash
$ sudo docker image pull hello-world
Using default tag: latest

(途中略)

Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

---
### docker image ls

ホストに存在するイメージの一覧を取得

```bash
$ sudo docker image ls
REPOSITORY                                        TAG   
          IMAGE ID            CREATED             SIZE
hello-world                                       latest
          fce289e99eb9        10 months ago       1.84kB
```

---
### docker container run

イメージからコンテナを作成＆実行

```bash
$ sudo docker container run hello-world

Hello from Docker!
This message shows that your installation appears to 
be working correctly.

(以下略)
```

- 実行結果が出力され、コンテナは終了する

---
### docker container ls

コンテナの一覧を取得

.zoom2[
```bash
$ sudo docker container ls -a
CONTAINER ID      IMAGE            COMMAND       CREATED      
    STATUS                     PORTS               NAMES
866926f90fec      hello-world      "/hello"      3 minutes ago
    Exited (0) 3 minutes ago                       nice_leakey
```
]

- オプションなしの場合は、実行中のコンテナのみ取得
- -a をつけることで全てのコンテナを取得できる

---
### docker container rm

指定したコンテナを削除

.zoom2[
```bash
$ sudo docker container rm nice_leakey
nice_leakey
```
]

- コンテナIDまたはコンテナ名で指定する
- hello-worldコンテナをすべて削除したい場合は、以下のように指定

.zoom2[
```bash
$ sudo docker container ls -a | grep hello-world | \
cut -d ' ' -f1 | sudo xargs docker container rm
```
]

---
### 

```bash

```

---
### 

```bash

```

---
### 

```bash

```

---
### 

```bash

```

---
### 

```bash

```

---
### 

```bash

```

---
class: center, middle, blue
## Dockerfile


---
class: center, middle, blue
## 管理ツール

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
