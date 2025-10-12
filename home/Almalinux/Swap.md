---
title: Almalinux -Swap-
description: Almalinux -Swap-
published: true
date: 2025-10-12T06:57:34.587Z
tags: almalinux, cmd, swap, メモリ
editor: markdown
dateCreated: 2025-10-12T06:57:34.587Z
---

# 💾 AlmaLinux Swap 完全ガイド

> **目的:**  
> このドキュメントでは、AlmaLinuxでの **Swap（スワップ）領域** の仕組み・必要性・作成や設定方法について詳しく解説します。

---

## 🧠 1. Swapとは？

**Swap（スワップ）** とは、  
物理メモリ（RAM）が不足したときに、一時的に **ディスク領域を仮想メモリとして利用** する仕組みのことです。

### 🔎 イメージ図

```
┌────────────┐
│ 物理メモリ(RAM) │ ← メインで使われる
└────────────┘
        ↓
┌────────────┐
│ Swap領域 (Disk) │ ← メモリ不足時に一時退避
└────────────┘
```

---

## 💬 2. なぜSwapが必要？

| 理由 | 説明 |
|------|------|
| **メモリ不足の補助** | 一時的にRAMが足りなくなったとき、プロセスが落ちるのを防ぐ |
| **安定性の確保** | サーバーで大量のプロセスが動作しても、クラッシュを回避 |
| **ハイバネーション機能** | 一部のLinux環境では、休止状態に必要 |

> 💡 **注意:**  
> Swapは「遅いメモリ代替」であり、RAMよりも圧倒的に遅いです。  
> **常にSwapが使われている状態** は、メモリ不足のサインです。

---

## ⚙️ 3. Swapの確認コマンド

### 現在のSwap状態を確認

```bash
swapon --show
```

または、`free -h` で確認できます：

```bash
free -h
```

出力例：

```
              total        used        free      shared  buff/cache   available
Mem:           3.8G        1.2G        2.0G        100M        600M        2.3G
Swap:          2.0G          0B        2.0G
```

---

## 🧩 4. Swapファイルの作成手順

### ① ファイルを作成

```bash
sudo fallocate -l 2G /swapfile
```

> 💡 `2G` の部分を変更すればサイズを指定可能（例：`1G`, `4G` など）

### ② パーミッションを変更

```bash
sudo chmod 600 /swapfile
```

### ③ Swap領域として設定

```bash
sudo mkswap /swapfile
```

### ④ 有効化

```bash
sudo swapon /swapfile
```

### ⑤ 確認

```bash
swapon --show
```

---

## 🔁 5. 永続化設定（再起動後も有効にする）

`/etc/fstab` に以下を追記します：

```
/swapfile none swap sw 0 0
```

---

## ❌ 6. Swapを無効化する

### 一時的に無効化

```bash
sudo swapoff /swapfile
```

### 完全に削除する

```bash
sudo swapoff /swapfile
sudo rm /swapfile
sudo sed -i '/\/swapfile/d' /etc/fstab
```

---

## ⚖️ 7. Swapサイズの目安

| RAM容量 | 推奨Swapサイズ |
|----------|----------------|
| 1GB未満 | RAMの2倍 |
| 2〜4GB | 同程度（RAMと同じ） |
| 8GB以上 | 2〜4GB程度 |
| 16GB以上 | 必要に応じて設定（0でも可） |

> 💬 サーバー用途では、メモリ監視を行い Swap使用率が高ければ RAM増設を検討しましょう。

---

## 🔧 8. スワップ動作の調整（swappiness）

Linuxには「どれくらい積極的にSwapを使うか」を示す設定値 **swappiness** があります。  
数値が低いほど「Swapを使わない」傾向になります。

### 現在値の確認

```bash
cat /proc/sys/vm/swappiness
```

### 一時的に変更（例：10に設定）

```bash
sudo sysctl vm.swappiness=10
```

### 永続的に変更

`/etc/sysctl.conf` に追記：

```
vm.swappiness=10
```

> 💡 一般的なサーバーでは `10〜20` 程度が推奨。

---

## 🧰 9. トラブルシューティング

| 症状 | 対処 |
|------|------|
| Swapが認識されない | `/etc/fstab` の設定ミスを確認 |
| Swapが異常に使用されている | `vm.swappiness` の値を下げる（例：10） |
| Swap作成時に「Permission denied」 | `chmod 600 /swapfile` を忘れていないか確認 |
| SSD上でSwapを使用している | 書き込み寿命に注意（`zram`の検討も可） |

---

## 🌈 10. まとめ

| ポイント | 内容 |
|-----------|------|
| Swapは「メモリの一時退避場所」 |
| 常用は避け、緊急時用と考える |
| `/swapfile` の作成と `/etc/fstab` 追記で永続化 |
| `swappiness` で使用頻度を調整可能 |
| SSD環境では慎重に設定 |

---

## 🔗 参考リンク

- [Red Hat 公式ドキュメント - Managing Swap Space](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_memory/managing-swap-space_configuring-and-managing-memory)
- [AlmaLinux 公式サイト](https://almalinux.org/)

---

> 💬 **Tip:**  
> Swapは「最後の保険」。  
> もしSwapが常に使われているなら、それはメモリ不足のサインです。 🚨
