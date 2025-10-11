---
title: Almalinux -Docler install-
description: Almalinux -Docler install-
published: true
date: 2025-10-11T22:13:47.638Z
tags: almalinux, docker
editor: markdown
dateCreated: 2025-10-11T22:10:07.207Z
---

# 🚀 AlmaLinuxでDocker & Docker Composeをカッコよくセットアップ！

AlmaLinuxで**Docker**と**Docker Compose**をインストールして、コンテナの力を最大限に引き出そう！
Dockerでアプリを軽快に動かし、Composeで複数コンテナをスマートに管理！✨

**前提**: AlmaLinux 8/9/10（RHEL互換）でroot権限（sudo）が必要です。システムは最新にしておきましょう。このガイドはインストールから基本的な使い方までカバー！

---

## 🐳 **Dockerとは**
Dockerは、アプリとその依存関係を**コンテナ**という軽量な仮想環境にパッケージ化。仮想マシンより高速で、開発から本番まで環境の差を気にせず動かせます。**Docker Compose**は、複数コンテナを`docker-compose.yml`で定義・管理する便利ツール！

---

## 🛠️ **ステップ1: システムの準備**
まず、システムを最新にしてスムーズなインストールを確保！

```bash
sudo dnf update -y
sudo reboot
```
*説明*: パッケージをアップデートし、再起動で反映。トラブル防止の基本ステップ。

---

## 📦 **ステップ2: Dockerのインストール**
AlmaLinuxのデフォルトリポジトリにDockerはないので、公式Docker CEリポジトリを追加してインストール。

### 2.1 **リポジトリの追加**
```bash
sudo dnf install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
*説明*: 
- `yum-utils`でリポ管理ツールを導入。
- Docker公式リポ（CentOS用だがAlmaLinuxで動作）を追加。

**注意**: AlmaLinux 10でも同じ手順でOK！

### 2.2 **Dockerパッケージのインストール**
Docker Engineと関連ツールを一気にインストール。

```bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
*説明*: 
- `docker-ce`: Dockerのコアエンジン。
- `docker-ce-cli`: コマンドラインインターフェース。
- `containerd.io`: コンテナランタイム。
- `docker-buildx-plugin`, `docker-compose-plugin`: ビルドとComposeをサポート。

GPGキーの確認が表示されたら`y`で進めてください。

### 2.3 **Dockerサービスの起動**
Dockerを起動し、システム起動時に自動でオンに。

```bash
sudo systemctl start docker
sudo systemctl enable docker
```
*説明*: 
- `start`: 今すぐDockerを起動。
- `enable`: ブート時に自動起動。

**確認**:
```bash
sudo systemctl status docker
```
*出力例*: `Active: active (running)`なら成功！

### 2.4 **非rootユーザーでの実行（オプション）**
sudoなしでDockerを使いたい場合、ユーザーをdockerグループに追加。

```bash
sudo usermod -aG docker $USER
newgrp docker
```
*説明*: 現在のユーザーをdockerグループに追加。ログアウト/ログインで反映。

### 2.5 **動作確認**
Dockerが動くかテスト！公式のサンプルコンテナを実行。

```bash
docker run hello-world
```
*出力例*:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```
*説明*: このメッセージが出ればDockerはバッチリ！

---

## 🎮 **ステップ3: Docker Composeのセットアップ**
Docker Composeは複数コンテナを一括管理するツール。AlmaLinuxでは**Docker公式のComposeプラグイン**を推奨（前ステップで`docker-compose-plugin`をインストール済み）。

### 3.1 **Composeの確認**
プラグインが正しく入っているかチェック。

```bash
docker compose version
```
*出力例*: `Docker Compose version v2.x.x`（バージョン番号が表示されればOK）。

**注意**: 古い`docker-compose`（Python版）ではなく、最新の`docker compose`（スペース区切り）が推奨。プラグイン版はDocker CLIに統合されメンテが楽！

### 3.2 **Composeが動かない場合（まれ）**
プラグインが動かない場合、バイナリを直接インストール。

```bash
sudo curl -SL https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
*説明*: 最新のComposeバイナリをダウンロード（バージョンは要確認）。`docker-compose version`で動作確認。

---

## 🧪 **ステップ4: Docker Composeの使い方例**
Composeの威力を試そう！Node.jsアプリ＋MySQLを連携するサンプル。

### 4.1 **プロジェクト構成**
以下のようなディレクトリを作成。

```bash
mkdir my-compose-app && cd my-compose-app
```

**Dockerfile**（Node.jsアプリ用）:
```dockerfile
FROM node:16
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

**簡単なNode.jsアプリ**（`server.js`）:
```javascript
const express = require('express');
const app = express();
app.get('/', (req, res) => res.send('Hello from Docker Compose!'));
app.listen(3000, () => console.log('Server running on port 3000'));
```

**package.json**:
```json
{
  "name": "my-compose-app",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

**docker-compose.yml**:
```yaml
version: '3.8'
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
      MYSQL_DATABASE: myapp
    volumes:
      - db-data:/var/lib/mysql
volumes:
  db-data:
```
*説明*: 
- `web`: Node.jsアプリをビルドして3000番ポートで公開。
- `db`: MySQLコンテナ。データはボリュームで永続化。

### 4.2 **Composeで起動**
```bash
docker compose up -d
```
*説明*: バックグラウンドでWebアプリとMySQLを一括起動。

### 4.3 **動作確認**
ブラウザで`http://localhost:3000`にアクセス。以下が表示されれば成功！

```
Hello from Docker Compose!
```

**コンテナ確認**:
```bash
docker compose ps
```
*説明*: 起動中のコンテナ（`web`と`db`）を確認。

**停止・削除**:
```bash
docker compose down
```
*説明*: コンテナとネットワークを停止・削除（ボリュームは残る）。

---

## ⚠️ **トラブルシューティングTips**
- **リポエラー**: `cat /etc/os-release`でAlmaLinuxバージョンを確認。8/9/10で手順は同じ。
- **Podman競合**: Podmanが邪魔なら`sudo dnf remove podman`。
- **ファイアウォール**: ポート開放が必要なら`sudo firewall-cmd --add-port=3000/tcp --permanent && sudo firewall-cmd --reload`。
- **Composeエラー**: `docker compose version`でエラーならプラグインを再インストール。
- **クリーンアップ**: 不要なイメージ/コンテナを削除`docker system prune -a`。

---

## 🌐 **参考サイト**
- [Docker公式インストールガイド](https://docs.docker.com/engine/install/centos/)
- [Docker Compose公式](https://docs.docker.com/compose/)
- [AlmaLinux公式Wiki](https://wiki.almalinux.org/)
- [Docker Hub](https://hub.docker.com/)

---
