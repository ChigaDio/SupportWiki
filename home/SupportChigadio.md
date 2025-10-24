---
title: SupportChigadio
description: Unity専用サポートひな型ツール
published: true
date: 2025-10-24T11:40:14.239Z
tags: c#, script, unity
editor: markdown
dateCreated: 2025-10-24T11:40:14.239Z
---

# **SupportChigadio**

## 📋 **概要**
SupportChigadioは、Unity向けの専用独自ツールです。主な機能は、C#のプログラミング自動生成のためのひな型ツールを提供することです。このツールを使用することで、開発者は効率的にコードの雛形を生成し、Unityプロジェクトの開発を加速できます。

リポジトリ: [https://github.com/ChigaDio/SupportChigadio](https://github.com/ChigaDio/SupportChigadio)

## 🛠️ **必要条件**
このツールを使用するには、以下の2つのUnityパッケージが必要です：
- **UniTask**
- **Addressables**

以下に、それぞれのインストール手順を説明します。

### 📦 **UniTaskのインストール**
UniTaskは、Unityでの非同期プログラミングを簡素化するためのライブラリです。以下の手順でインストールしてください：

1. **Unity Package Managerを開く**
   - Unityエディタのメニューから「Window」→「Package Manager」を選択。
2. **UniTaskを検索**
   - Package Managerウィンドウで「Add package from git URL」をクリック。
   - 以下のURLを入力：
     ```
     https://github.com/Cysharp/UniTask.git?path=/Assets/Plugins/UniTask
     ```
3. **インストール**
   - 「Add」をクリックしてインストールを完了。
   - インストール後、UnityプロジェクトにUniTaskが追加されます。

### 📦 **Addressablesのインストール**
Addressablesは、Unityでアセット管理を効率化するためのシステムです。以下の手順でインストールしてください：

1. **Unity Package Managerを開く**
   - Unityエディタのメニューから「Window」→「Package Manager」を選択。
2. **Addressablesを検索**
   - Package Managerウィンドウで「Packages: Unity Registry」を選択。
   - 検索バーで「Addressables」を検索。
3. **インストール**
   - Addressablesパッケージを選択し、「Install」をクリック。
   - インストール後、Unityエディタのメニューに「Addressables Groups」ウィンドウが追加されます。
4. **初期設定**
   - 初回使用時に「Addressables Groups」ウィンドウを開き、指示に従って初期化を行ってください。

## 🚀 **基本的な使い方**
SupportChigadioツールの使用方法は以下の通りです：

1. **アプリの起動**
   - リポジトリに含まれる`app.exe`をダウンロードし、実行します。
2. **ツールにアクセス**
   - ウェブブラウザ（Google Chromeなど）を開き、以下のURLにアクセス：
     ```
     http://localhost:8000
     ```
   - これにより、ツールのインターフェースが表示されます。
3. **ツールの操作**
   - ツールのインターフェースに従って、C#のコード雛形を生成します。
   - 生成されたコードは、Unityプロジェクトに統合して使用できます。

## 📥 **リポジトリのクローン**
リポジトリをクローンする場合、以下のコマンドを使用してください：
```bash
git clone https://github.com/ChigaDio/SupportChigadio.git
```

## ⚠️ **注意事項**
- ツールを使用する前に、UnityプロジェクトにUniTaskとAddressablesが正しくインストールされていることを確認してください。
- ブラウザで`localhost:8000`にアクセスできない場合、`app.exe`が正しく実行されているか、ポートが競合していないかを確認してください。

## 📜 **ライセンス**
このリポジトリのライセンスについては、リポジトリ内の`LICENSE`ファイルを参照してください。