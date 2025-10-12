---
title: C# -strategy-
description: C# -strategy-
published: true
date: 2025-10-12T00:43:15.907Z
tags: c#, script, designpatterns, strategy
editor: markdown
dateCreated: 2025-10-12T00:43:15.907Z
---

# ストラテジーパターン in C#

## 概要
ストラテジーパターンは、アルゴリズムをカプセル化し、実行時に動的に切り替える行動デザインパターンです。アルゴリズムを独立させ、柔軟に変更可能にします。ゲーム開発では、AIの行動や攻撃方法の切り替えに適しています。

---

## ストラテジーパターンの特徴
- **アルゴリズムのカプセル化**: アルゴリズムを独立したクラスとして定義。
- **動的な切り替え**: 実行時にアルゴリズムを変更可能。
- **拡張性**: 新しいアルゴリズムを簡単に追加可能。

---

## 実装方法
以下は、C#でのストラテジーパターンの実装例です。

```csharp
// ストラテジーインターフェース
public interface IStrategy
{
    void Execute();
}

// 具体的なストラテジー
public class StrategyA : IStrategy
{
    public void Execute() => Console.WriteLine("Executing Strategy A");
}

public class StrategyB : IStrategy
{
    public void Execute() => Console.WriteLine("Executing Strategy B");
}

// コンテキスト
public class Context
{
    private IStrategy _strategy;

    public Context(IStrategy strategy)
    {
        _strategy = strategy;
    }

    public void SetStrategy(IStrategy strategy)
    {
        _strategy = strategy;
    }

    public void ExecuteStrategy()
    {
        _strategy.Execute();
    }
}
```

### 使用例
```csharp
Context context = new Context(new StrategyA());
context.ExecuteStrategy(); // 出力: Executing Strategy A

context.SetStrategy(new StrategyB());
context.ExecuteStrategy(); // 出力: Executing Strategy B
```

---

## メリット
1. **柔軟性**: 実行時にアルゴリズムを動的に変更可能。
2. **再利用性**: ストラテジーオブジェクトを複数のコンテキストで再利用可能。
3. **単一責任の原則**: アルゴリズムを独立させ、コードの保守性を向上。

## デメリット
1. **クラスの増加**: 各アルゴリズムごとにクラスが必要。
2. **クライアントの負担**: クライアントが適切なストラテジーを選択する必要がある。

---

## ゲーム開発での使用例
ゲーム開発では、敵のAI行動やプレイヤーの攻撃方法を切り替える際に有用です。

### 例: 敵の攻撃ストラテジー（Unity向け）
```csharp
using UnityEngine;

// 攻撃ストラテジーインターフェース
public interface IAttackStrategy
{
    void Attack();
}

// 具体的なストラテジー
public class MeleeAttack : IAttackStrategy
{
    public void Attack() => Debug.Log("Performing melee attack!");
}

public class RangedAttack : IAttackStrategy
{
    public void Attack() => Debug.Log("Performing ranged attack!");
}

public class Enemy : MonoBehaviour
{
    private IAttackStrategy _attackStrategy;

    public void SetAttackStrategy(IAttackStrategy strategy)
    {
        _attackStrategy = strategy;
    }

    public void PerformAttack()
    {
        _attackStrategy?.Attack();
    }
}

public class GameController : MonoBehaviour
{
    void Start()
    {
        Enemy enemy = gameObject.AddComponent<Enemy>();

        enemy.SetAttackStrategy(new MeleeAttack());
        enemy.PerformAttack(); // 出力: Performing melee attack!

        enemy.SetAttackStrategy(new RangedAttack());
        enemy.PerformAttack(); // 出力: Performing ranged attack!
    }
}
```

---

## 注意点
- **ストラテジーの設計**: ストラテジーインターフェースを適切に設計しないと、拡張性が損なわれる。
- **Unityでの考慮**: ストラテジーオブジェクトをプレハブやスクリプタブルオブジェクトで管理すると効率的。
- **パフォーマンス**: 頻繁なストラテジー切り替えによるオーバーヘッドに注意。

---

## まとめ
ストラテジーパターンは、アルゴリズムの動的な切り替えが必要な場合に有用です。ゲーム開発では、AIや攻撃方法の柔軟な変更に適しており、コードの再利用性と拡張性を高めます。