---
title: Enum ID
description: Enumの自動生成
published: true
date: 2025-10-24T11:56:46.830Z
tags: c#, unity, tool
editor: markdown
dateCreated: 2025-10-24T11:56:46.830Z
---

# 📋 **Enum IDの使い方**

このドキュメントでは、SupportChigadioツールの「Enum ID」機能の使い方と、自動生成されるC#コードおよびその拡張クラスの使用方法について説明します。

## 🚀 **Enum IDの基本的な使い方**

以下の手順で、Enum IDを使用してC#のEnumコードを生成できます。

1. **ツールにアクセス**
   - リポジトリに含まれる`app.exe`を実行します。
   - ウェブブラウザ（Google Chromeなど）で以下のURLにアクセス：
     ```
     http://localhost:8000
     ```

2. **Enum IDを選択**
   - 画面左側のメニューから「Generate Tool」を選択。
   - 「Enum ID」をクリックすると、現在定義されているEnumのリストが表示されます。

3. **新しいEnumの作成**
   - 新しいEnumを作成するには、上部の「CREATE NEW ENUM」ボタンをクリック。
   - Enumの名前を入力（例: `ScenarioCharacterPositionID`）。
   - 入力後、リストに新しく追加されたEnumが表示されます。

4. **Enumの編集**
   - リストから作成したEnumの名前（例: `ScenarioCharacterPositionID`）をクリック。
   - ページが移動し、以下の項目を編集可能：
     - **レコード数**: Enumに含まれる項目の数。
     - **各レコードの定義名**: 各項目の名前（例: `None`, `Left`, `Right`）。
   - **注意**: Enumの値は基本的に連番（0から始まる整数）が想定されています。
   ![enum01.png](/enum01.png)

5. **操作ボタン**
   - **新しい行作成**: 新しいレコードを1行追加します。
   - **デフォルト作成**: 指定したテンプレートに基づいて複数のレコードを一括追加します。
   - **保存**: 現在の状態を保存します。
   - **C#生成**: EnumのC#コードを自動生成します。
   - **削除**: 選択したEnumを削除します。

6. **生成されたコードの確認**
   - 生成されたC#コードは、リポジトリをクローンしたディレクトリの1つ上の階層に自動作成される`data/enum`フォルダ内に保存されます。
   - 各Enumごとに専用のフォルダ（例: `ScenarioCharacterPositionID`）が作成され、その中にコードが保存されます。

## 📝 **生成されるC#コードの例**

以下は、生成されるEnumコードの例です：

```csharp
namespace GameCore.Enums
{
    public enum ScenarioCharacterPositionID
    {
        None = 0, // デフォルト値
        Left = 1, // 左位置
        Right = 2, // 右位置
        Max = 3
    }
}
```

- **説明**:
  - `None`: デフォルト値（通常0）。
  - `Left`, `Right`: 具体的なEnum値（連番）。
  - `Max`: Enumの最大値を示す（ループ処理などで使用）。

## 🔧 **拡張クラスの使い方**

SupportChigadioは、Enum用の便利な拡張メソッドを含む`ScenarioCharacterPositionIDExtensions`クラスも自動生成します。以下は生成されるコードとその使い方の説明です：

```csharp
using System;
using UnityEngine;
using System.Collections.Generic;

namespace GameCore.Enums
{
    public static class ScenarioCharacterPositionIDExtensions
    {
        // Enum値をintに変換
        public static int ToInt(this ScenarioCharacterPositionID id)
        {
            return (int)id;
        }

        // int値をEnumに変換
        public static ScenarioCharacterPositionID ToScenarioCharacterPositionID(this int id)
        {
            return (ScenarioCharacterPositionID)id;
        }

        // Enum値をインデックス（0始まり）に変換
        public static int ToIndex(this ScenarioCharacterPositionID id)
        {
            return (int)id - 1;
        }

        // Enum値を順に処理する
        public static void ForID(Action<ScenarioCharacterPositionID> action)
        {
            if (action == null) throw new ArgumentNullException(nameof(action));
            for (EnumIDIter<ScenarioCharacterPositionID> id = ScenarioCharacterPositionID.Left; id < ScenarioCharacterPositionID.Max; id++)
            {
                action(id);
            }
        }

        // 条件に一致するすべてのEnum値をリストで取得
        public static List<ScenarioCharacterPositionID> FindAll(Func<ScenarioCharacterPositionID, bool> predicate)
        {
            if (predicate == null) throw new ArgumentNullException(nameof(predicate));

            var results = new List<ScenarioCharacterPositionID>();
            for (EnumIDIter<ScenarioCharacterPositionID> id = ScenarioCharacterPositionID.Left; id < ScenarioCharacterPositionID.Max; id++)
            {
                ScenarioCharacterPositionID value = id;
                if (!Enum.IsDefined(typeof(ScenarioCharacterPositionID), value))
                    continue; // 無効な値はスキップ
                if (predicate(value))
                    results.Add(value);
            }
            return results;
        }

        // 条件に一致する最初のEnum値を取得
        public static ScenarioCharacterPositionID Find(Func<ScenarioCharacterPositionID, bool> predicate)
        {
            if (predicate == null) throw new ArgumentNullException(nameof(predicate));

            for (EnumIDIter<ScenarioCharacterPositionID> id = ScenarioCharacterPositionID.Left; id < ScenarioCharacterPositionID.Max; id++)
            {
                ScenarioCharacterPositionID value = id;
                if (!Enum.IsDefined(typeof(ScenarioCharacterPositionID), value))
                    continue; // 無効な値はスキップ
                if (predicate(value))
                    return value;
            }
            return ScenarioCharacterPositionID.None; // デフォルト値
        }
    }
}
```

### **各メソッドの使い方**

1. **`ToInt`**
   - **用途**: Enum値を`int`型に変換。
   - **使用例**:
     ```csharp
     ScenarioCharacterPositionID position = ScenarioCharacterPositionID.Left;
     int value = position.ToInt(); // 結果: 1
     Debug.Log(value); // Unityでログ出力
     ```

2. **`ToScenarioCharacterPositionID`**
   - **用途**: `int`値を対応するEnum値に変換。
   - **使用例**:
     ```csharp
     int value = 2;
     ScenarioCharacterPositionID position = value.ToScenarioCharacterPositionID(); // 結果: Right
     Debug.Log(position); // Unityでログ出力
     ```

3. **`ToIndex`**
   - **用途**: Enum値を0始まりのインデックスに変換（`None`を除外）。
   - **使用例**:
     ```csharp
     ScenarioCharacterPositionID position = ScenarioCharacterPositionID.Right;
     int index = position.ToIndex(); // 結果: 1 (Rightは2から1を引いた値)
     Debug.Log(index); // Unityでログ出力
     ```

4. **`ForID`**
   - **用途**: Enum値を`Left`から`Max`未満まで順に処理。
   - **使用例**:
     ```csharp
     ScenarioCharacterPositionIDExtensions.ForID(id =>
     {
         Debug.Log($"Processing: {id}"); // Left, Rightを順に出力
     });
     ```

5. **`FindAll`**
   - **用途**: 条件に一致するすべてのEnum値をリストで取得。
   - **使用例**:
     ```csharp
     var validPositions = ScenarioCharacterPositionIDExtensions.FindAll(id => id != ScenarioCharacterPositionID.None);
     foreach (var pos in validPositions)
     {
         Debug.Log(pos); // Left, Rightを出力
     }
     ```

6. **`Find`**
   - **用途**: 条件に一致する最初のEnum値を取得。見つからない場合は`None`を返す。
   - **使用例**:
     ```csharp
     var position = ScenarioCharacterPositionIDExtensions.Find(id => id == ScenarioCharacterPositionID.Right);
     Debug.Log(position); // Rightを出力
     ```

## ⚠️ **注意事項**
- Enum値は連番（0始まり）を前提とした設計です。カスタム値を設定する場合は、生成コードの動作を確認してください。
- 生成されたコードは`data/enum`フォルダに保存されます。リポジトリをクローンしたディレクトリ構造を確認してください。
- 拡張クラス内の`EnumIDIter<T>`は、ツールが提供する専用イテレータ型です。Unity環境で動作するように設計されています。