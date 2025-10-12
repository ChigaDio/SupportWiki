---
title: Almalinux -FireWall-
description: Almalinuc --Firewall 
published: true
date: 2025-10-12T06:47:38.920Z
tags: almalinux, firewall, cmd
editor: markdown
dateCreated: 2025-10-12T06:47:38.920Z
---

# 🧱 AlmaLinux Firewall 完全ガイド

> **目的:**  
> このドキュメントは、AlmaLinuxで `firewalld` を使用してポート開放・設定変更・再読み込みを行うためのまとめです。  
> また、「そもそもFirewallとは何か？」についても初心者向けに解説しています。

---

## 🌐 1. Firewallとは？

**Firewall（ファイアウォール）**とは、  
「サーバーやPCをインターネット上の不正アクセスから守る**防御壁**」のことです。

### 🔒 役割
| 役割 | 説明 |
|------|------|
| **通信制御** | 指定したポート・IPのみ通信を許可／拒否する |
| **セキュリティ強化** | 攻撃者が侵入するリスクを減らす |
| **可視化** | どのサービスが通信しているかを把握できる |

### 🚫 なぜ必要？
サーバーは常に外部からアクセスされる可能性があります。  
ファイアウォールを設定しないと、**意図しない通信（ハッキング・スキャン・不正侵入）**を受ける危険性があります。

---

## ⚙️ 2. firewalldとは？

AlmaLinux（およびRHEL系）では、標準で **firewalld** というファイアウォール管理サービスが利用されます。

- **バックエンド:** iptables / nftables  
- **設定管理:** ゾーン（zone）とサービス（service）という概念で管理  
- **操作ツール:** `firewall-cmd` コマンド

---

## 🧩 3. firewalldの基本コマンド

### 🔄 サービスの起動・停止・状態確認

```bash
# firewalldの起動
sudo systemctl start firewalld

# 自動起動を有効化（再起動後も有効）
sudo systemctl enable firewalld

# 状態確認
sudo systemctl status firewalld

# 一時的に停止
sudo systemctl stop firewalld

# 無効化（自動起動オフ）
sudo systemctl disable firewalld
```

---

## 🚪 4. ポートの開放・閉鎖

### 🔓 ポートを開放（例: TCPの80番ポート）

```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```

### 🔒 ポートを閉じる

```bash
sudo firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

> 💡 **--permanent** を付けない場合、一時的な変更（再起動でリセット）になります。

### 🔁 設定の再読み込み（反映）

```bash
sudo firewall-cmd --reload
```

---

## 🧾 5. 設定の確認

### 🔍 開放済みポートの一覧

```bash
sudo firewall-cmd --zone=public --list-ports
```

### 🧱 許可済みサービス一覧

```bash
sudo firewall-cmd --zone=public --list-services
```

### 🌍 現在のゾーンと有効インターフェースの確認

```bash
sudo firewall-cmd --get-active-zones
```

---

## ⚡ 6. よく使う設定例

| 目的 | コマンド例 |
|------|-------------|
| HTTP(80) と HTTPS(443) を開放 | `sudo firewall-cmd --add-service=http --add-service=https --permanent` |
| SSH(22) のみ許可 | `sudo firewall-cmd --zone=public --add-service=ssh --permanent` |
| 特定のポート範囲を開放（例: 3000〜3100） | `sudo firewall-cmd --add-port=3000-3100/tcp --permanent` |
| 設定反映 | `sudo firewall-cmd --reload` |
| 現在のゾーン設定確認 | `sudo firewall-cmd --list-all` |

---

## 🧭 7. firewalldのゾーンとは？

| ゾーン名 | 特徴 |
|-----------|------|
| **public** | 一般的な外部アクセス向け。信頼されていないネットワーク用（デフォルト） |
| **home** | 家庭LANなど比較的安全なネットワーク |
| **internal** | 内部システム間通信向け |
| **trusted** | すべての通信を許可（注意！） |
| **block/drop** | すべての通信を遮断 |

### 🔧 デフォルトゾーンを確認・変更

```bash
# 現在のゾーン確認
sudo firewall-cmd --get-default-zone

# デフォルトゾーンを変更（例: internal）
sudo firewall-cmd --set-default-zone=internal
```

---

## 🧰 8. トラブルシューティング

| 症状 | 対処 |
|------|------|
| ポート開放したのにアクセスできない | `sudo firewall-cmd --reload` を忘れていないか確認 |
| サービスは動いているが接続できない | `sudo systemctl status firewalld` で状態確認 |
| 特定のIPのみ許可したい | `--add-rich-rule` を使用（下記参照） |

### 🌐 特定IPのみ許可する例

```bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.0.10" port port=8080 protocol=tcp accept'
sudo firewall-cmd --reload
```

---

## 🛡️ 9. まとめ

| ポイント | 内容 |
|-----------|------|
| Firewallは「通信を守る防御壁」 |
| AlmaLinuxでは `firewalld` を使用 |
| `firewall-cmd` でポートやサービスを管理 |
| 設定変更後は `--reload` が必須 |
| 安易に `trusted` ゾーンを使わない！ |

---

## 🌈 10. 参考リンク

- 🔗 [Red Hat firewalld 公式ドキュメント](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/security_and_compliance/using-and-configuring-firewalls_security-and-compliance)
- 🔗 [firewalld.org (英語)](https://firewalld.org/)
- 🔗 [AlmaLinux 公式サイト](https://almalinux.org/)

---

> 💬 **Tip:**  
> セキュリティは「閉じるのが基本、開けるのは必要最小限」。  
> 不要なポートを開けっぱなしにしないようにしましょう 🔐


