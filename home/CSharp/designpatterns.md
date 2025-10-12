---
title: C# -designpatterns-
description: C# -designpatterns-
published: true
date: 2025-10-12T00:52:41.197Z
tags: c#, script, designpatterns
editor: markdown
dateCreated: 2025-10-12T00:52:41.197Z
---



## 概要
デザインパターンとは、ソフトウェア設計における一般的な問題に対する再利用可能な解決策のテンプレートです。1994年に出版された「Design Patterns: Elements of Reusable Object-Oriented Software」（通称：GoF本）で、Erich Gammaらによって23のパターンが体系化されました。これらは、オブジェクト指向プログラミングにおける設計のベストプラクティスを提供し、コードの再利用性、保守性、拡張性を高めます。

---

## デザインパターンの目的
デザインパターンは、以下のような目的で使用されます：
- **再利用性の向上**：共通の問題に対する標準化された解決策を提供。
- **コードの構造化**：設計を整理し、読みやすく保守しやすいコードを実現。
- **柔軟性の確保**：システムの拡張や変更を容易に。
- **コミュニケーションの円滑化**：開発者間で共通の「設計の言語」を提供。

---

## デザインパターンの分類
GoF本では、デザインパターンを以下の3つのカテゴリに分類しています：

### 1. 生成パターン（Creational Patterns）
オブジェクトの生成メカニズムを扱うパターン。生成プロセスを抽象化し、柔軟性と再利用性を高めます。
- **例**：
  - [シングルトン](https://wiki-heroku-9e9k.onrender.com/ja/home/CSharp/designpatterns/singleton)：インスタンスを1つだけに制限。
  - [ファクトリメソッド](https://wiki-heroku-9e9k.onrender.com/ja/home/CSharp/designpatterns/factory)：サブクラスにオブジェクト生成を委譲。
  - 抽象ファクトリ：関連するオブジェクト群を生成。
  - [ビルダー](https://wiki-heroku-9e9k.onrender.com/ja/home/CSharp/designpatterns/builder)：複雑なオブジェクトを段階的に構築。
  - [プロトタイプ](https://wiki-heroku-9e9k.onrender.com/ja/home/CSharp/designpatterns/prototype)：既存オブジェクトをコピーして生成。

### 2. 構造パターン（Structural Patterns）
クラスやオブジェクトの構成を扱うパターン。システムの構造を効率的かつ柔軟に構築します。
- **例**：
  - アダプター：互換性のないインターフェースを適応。
  - デコレーター：オブジェクトに動的に機能を追加。
  - ファサード：複雑なサブシステムを簡素化。
  - コンポジット：オブジェクトを木構造で扱う。

### 3. 行動パターン（Behavioral Patterns）
オブジェクト間の通信や責任分担を扱うパターン。オブジェクト間の連携を効率化します。
- **例**：
  - [オブザーバー](https://wiki-heroku-9e9k.onrender.com/ja/home/CSharp/designpatterns/observer)：状態変化を複数のオブジェクトに通知。
  - [ストラテジー](https://wiki-heroku-9e9k.onrender.com/ja/home/CSharp/designpatterns/strategy)：アルゴリズムを動的に切り替え。
  - コマンド：操作をオブジェクトとしてカプセル化。
  - テンプレートメソッド：アルゴリズムの骨格を定義。

---

## メリット
1. **再利用性**：共通の問題に対する標準化された解決策を提供。
2. **保守性の向上**：コードが整理され、変更やデバッグが容易に。
3. **拡張性**：新しい要件に対応しやすく、システムのスケーラビリティを向上。
4. **チーム開発の効率化**：開発者間で共通の設計言語を共有。

## デメリット
1. **学習コスト**：初心者にとって、パターンの理解と適用が難しい場合がある。
2. **過剰な適用**：不適切な場面で使用すると、コードが複雑化する。
3. **オーバーヘッド**：単純な問題に対してパターンを適用すると、コード量やパフォーマンスに影響。

---

## ゲーム開発での使用例
ゲーム開発では、デザインパターンは以下のような場面で活用されます：

### 例1: シングルトン（ゲームマネージャー）
ゲーム全体の状態（スコア、レベル、進行状況）を管理する`GameManager`をシングルトンとして実装。シーン間で一貫したデータアクセスを保証。

```csharp
public class GameManager
{
    private static GameManager _instance;
    public static GameManager Instance => _instance ??= new GameManager();
    private GameManager() { }
    public int Score { get; set; }
}
```

### 例2: オブザーバー（UI更新）
プレイヤーのHPが変化した際に、UIや他のシステムに通知。イベント駆動型の更新を実現。

```csharp
public interface IHealthObserver
{
    void OnHealthChanged(int health);
}

public class Player
{
    private List<IHealthObserver> _observers = new();
    private int _health;
    public int Health
    {
        get => _health;
        set
        {
            _health = value;
            foreach (var observer in _observers)
                observer.OnHealthChanged(_health);
        }
    }
}
```

### 例3: ストラテジー（AI行動）
敵の行動パターンを動的に切り替え。たとえば、近接攻撃や遠距離攻撃を状況に応じて変更。

```csharp
public interface IAttackStrategy
{
    void Attack();
}

public class Enemy
{
    private IAttackStrategy _strategy;
    public void SetStrategy(IAttackStrategy strategy) => _strategy = strategy;
    public void PerformAttack() => _strategy.Attack();
}
```

---

## 注意点
- **適切なパターンの選択**：問題に合ったパターンを選ばないと、コードが複雑化。
- **ゲーム開発での考慮**：Unityなどでは、MonoBehaviourの制約やパフォーマンスを考慮（例：`new`の制限、`Instantiate`の活用）。
- **バランスの重要性**：パターンを過剰に使用すると、コードが冗長になるため、シンプルさを保つ。

---

## まとめ
デザインパターンは、ソフトウェア設計における再利用可能な解決策であり、コードの構造化、保守性、拡張性を向上させます。ゲーム開発では、シングルトン、オブザーバー、ストラテジーなどが特に有用で、Unityの特性を活かした実装が求められます。適切なパターンを選び、状況に応じて適用することで、効率的で高品質な開発を実現できます。