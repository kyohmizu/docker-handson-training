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
- Docker軽く触ったことある人
- コンテナって…？な人

---
### 今日のゴール

- コンテナについてある程度理解する
- Dockerfileを書けるようになる
- Dockerコンテナをデプロイできるようになる

---
### アウトライン

1. Docker概要
1. 環境確認
1. Dockerコマンド
1. Dockerfile作成
1. サンプルアプリデプロイ
1. オーケストレーション

---
class: center, middle, blue
## Docker概要

---
### コンテナ？

---
### Docker？

---
### Pipeline

.zoom0[
<u><https://github.com/starkandwayne/concourse-tutorial/blob/master/tutorials/basic/job-inputs/pipeline.yml></u>

```yaml
resources:
  - name: resource-tutorial
    type: git
    source:
      uri: https://github.com/starkandwayne/concourse-tutorial.git
      branch: develop

  - name: resource-app
    type: git
    source:
      uri: https://github.com/cloudfoundry-community/simple-go-web-app.git

jobs:
  - name: job-test-app
    public: true
    plan:
      - get: resource-tutorial
      - get: resource-app
        trigger: true
      - task: web-app-tests
        file: resource-tutorial/tutorials/basic/job-inputs/task_run_tests.yml
```
]

---
class: header-margin
### Pipeline

<center><img src="concourse-p.png" width=110%></center>

---
### Argo CD

<u><https://argoproj.github.io/argo-cd/></u>

.left-large[
- Declarative, GitOps CD tool

- Automated deployment of desired application states

- Support for config management tools  
(Kustomize, Helm etc)
]

.right-small[
    <center><img src="https://argoproj.github.io/static/argo-wheel.23b3ad84.png" width=70%></center>
]

---
### 

---

---
class: center, middle, blue
# 参考

Docker pull commit push Dockerfile WordPress環境をつくる
<u><https://qiita.com/cyberblack28/items/7ed6fbc75503bf9418b3></u>

---
class: center, middle, blue
# おわり
