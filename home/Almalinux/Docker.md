---
title: Almalinux -Docker-
description: Almalinux -Docker-
published: true
date: 2025-10-11T22:16:56.687Z
tags: almalinux, docker
editor: markdown
dateCreated: 2025-10-11T22:04:09.906Z
---

# 🚀 Dockerについて

Dockerを解説！

---

## 🐳 **Dockerとは**
Dockerは、アプリケーションを**コンテナ**という軽量な箱に詰め込み、どんな環境でもサクサク動かせるプラットフォーム。仮想マシンより軽量で、開発から本番まで「環境の違い」を気にせずアプリを展開可能！🛠️

---



---

## 📜 Dockerに関するMemoPage

- [**Docker Install:基本インストール**](https://wiki-heroku-9e9k.onrender.com/ja/home/Almalinux/Docker/Almalinux_Docler_install)

---

## 🧠 **Dockerの概念**
Dockerの基本を押さえよう！以下のキーワードが核心です：

- **コンテナ**  
  アプリとその依存関係（ライブラリや設定）を1つの箱にまとめたもの。OSはホストと共有するので超軽量！  
  *例*: NginxやPythonアプリをコンテナで動かす。

- **イメージ**  
  コンテナの設計図。アプリや環境をパッケージ化したテンプレート。  
  *例*: `nginx:latest`や`python:3.9`。

- **Dockerfile**  
  イメージを作るための「レシピ」。どんなアプリや設定を入れるか記述。  
  *例*: 必要なライブラリをインストールしてアプリを起動。

- **レジストリ**  
  イメージを保存・共有する倉庫。Docker Hubが有名！  
  *例*: `docker pull mysql`でMySQLイメージをゲット。

- **Docker Engine**  
  Dockerの心臓。コンテナの作成や管理を担う。

---

## 📖 **Dockerの説明**
Dockerは**コンテナ技術**を活用し、開発環境の統一やアプリの移植性を劇的に向上させます。仮想マシンと違い、ホストOSを共有するから**高速かつ省リソース**。以下のようなシーンで大活躍：
- **開発環境の統一**: ローカルと本番で「動かない」を防ぐ。
- **CI/CD**: ビルド・テスト・デプロイをスムーズに。
- **マイクロサービス**: 各サービスを独立したコンテナで管理。

---

## ✨ **Dockerの機能例**
Dockerの便利な機能をコード付きで紹介！実例で使い方をマスターしよう！🔥

### 1. **コンテナの作成・実行**  
Nginxを動かしてWebサーバーを即立ち上げ！  
```bash
docker run -d -p 80:80 nginx
```
*説明*: バックグラウンドでNginxコンテナを起動。ホストの80番ポートをコンテナの80番に接続。

---

### 2. **カスタムイメージのビルド**  
Pythonアプリ用のイメージを作成！  
**Dockerfile**:
```dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```
**ビルドコマンド**:
```bash
docker build -t my-python-app .
```
*説明*: Python 3.9をベースに、アプリのコードと依存関係をイメージにまとめる。

---

### 3. **コンテナの管理**  
コンテナをサクッと操作！  
```bash
docker ps           # 実行中のコンテナ一覧
docker stop <id>    # コンテナ停止
docker rm <id>      # コンテナ削除
```
*説明*: コンテナの状態確認や停止・削除を簡単に行える。

---

### 4. **ネットワーク設定**  
コンテナ同士を繋ぐ！  
```bash
docker network create my-network
docker run --network my-network --name db -d mysql
```
*説明*: カスタムネットワークでMySQLコンテナを立ち上げ、他のコンテナと通信可能に。

---

### 5. **データ永続化（ボリューム）**  
Redisのデータをホストに保存！  
```bash
docker volume create my-volume
docker run -v my-volume:/data -d redis
```
*説明*: コンテナが消えてもデータはボリュームで安全に保存。

---

## 🛠️ **Dockerの仕様例**
Node.jsアプリをDockerで動かす例を**カッコよく**紹介！実践的なコードでイメージを掴もう！😎

### **例1: Node.jsアプリのDockerfile**
**Dockerfile**:
```dockerfile
FROM node:16
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```
**実行**:
```bash
docker build -t my-node-app .         # イメージをビルド
docker run -d -p 3000:3000 my-node-app # コンテナ起動
```
*説明*: Node.jsアプリを3000番ポートで公開。ブラウザで`localhost:3000`にアクセス！

---

### **例2: Docker Composeで複数コンテナ**
**docker-compose.yml**:
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db-data:/var/lib/mysql
volumes:
  db-data:
```
**起動**:
```bash
docker-compose up -d
```
*説明*: Node.jsアプリとMySQLを連携。Composeで一括管理！

---

## 🌐 **Dockerに関するサイトURL**
- [公式サイト](https://www.docker.com/)  
- [Docker Hub](https://hub.docker.com/)  
- [公式ドキュメント](https://docs.docker.com/)  
- [GitHub](https://github.com/docker)

---
