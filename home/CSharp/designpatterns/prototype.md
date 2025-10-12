---
title: C# -ProtoType-
description: C# -ProtoType-
published: true
date: 2025-10-12T00:39:20.813Z
tags: c#, script, designpatterns, prototype
editor: markdown
dateCreated: 2025-10-12T00:39:20.813Z
---

# プロトタイプパターン in C#

## 概要
プロトタイプパターンは、既存のオブジェクトをコピー（クローン）して新しいオブジェクトを生成する生成デザインパターンです。オブジェクトの複雑な初期化を避け、効率的にインスタンスを生成します。ゲーム開発では、敵やアイテムの複製に適しています。

---

## プロトタイプパターンの特徴
- **オブジェクトのコピー**: 既存のオブジェクトを基に新しいインスタンスを生成。
- **初期化コストの削減**: 複雑な構築プロセスを回避。
- **柔軟性**: 異なる状態のオブジェクトを簡単に複製可能。

---

## 実装方法
C#では、`ICloneable`インターフェースを利用してプロトタイプパターンを実装できます。

```csharp
public interface IPrototype
{
    IPrototype Clone();
}

public class ConcretePrototype : IPrototype
{
    public string Name { get; set; }
    public int Value { get; set; }

    public ConcretePrototype(string name, int value)
    {
        Name = name;
        Value = value;
    }

    public IPrototype Clone()
    {
        return new ConcretePrototype(Name, Value); // 浅いコピー
    }

    public void Display()
    {
        Console.WriteLine($"Prototype: Name={Name}, Value={Value}");
    }
}
```

### 使用例
```csharp
ConcretePrototype original = new ConcretePrototype("PrototypeA", 100);
original.Display(); // 出力: Prototype: Name=PrototypeA, Value=100

IPrototype clone = original.Clone();
clone.Display(); // 出力: Prototype: Name=PrototypeA, Value=100
```

---

## メリット
1. **効率的な生成**: 複雑な初期化を回避し、オブジェクトを迅速に生成。
2. **柔軟性**: 異なる状態のオブジェクトを簡単に複製可能。
3. **再利用性**: 既存のオブジェクトを再利用して新しいインスタンスを作成。

## デメリット
1. **深いコピーの問題**: 複雑なオブジェクトの場合、深いコピーを実装する必要がある。
2. **実装の複雑さ**: クローンのロジックが複雑になる場合がある。

---

## ゲーム開発での使用例
ゲーム開発では、敵やアイテムを効率的に複製する際にプロトタイプパターンが有用です。

### 例: 敵のクローン生成（Unity向け）
```csharp
using UnityEngine;

public interface IEnemyPrototype
{
    IEnemyPrototype Clone();
    void Attack();
}

public class Enemy : MonoBehaviour, IEnemyPrototype
{
    public string EnemyType { get; set; }
    public int Health { get; set; }

    public void Attack() => Debug.Log($"{EnemyType} attacks!");

    public IEnemyPrototype Clone()
    {
        GameObject clone = Instantiate(gameObject);
        Enemy enemy = clone.GetComponent<Enemy>();
        enemy.EnemyType = EnemyType;
        enemy.Health = Health;
        return enemy;
    }
}

public class GameController : MonoBehaviour
{
    void Start()
    {
        GameObject enemyObj = new GameObject("Enemy");
        Enemy original = enemyObj.AddComponent<Enemy>();
        original.EnemyType = "Goblin";
        original.Health = 50;
        original.Attack(); // 出力: Goblin attacks!

        IEnemyPrototype clone = original.Clone();
        clone.Attack(); // 出力: Goblin attacks!
    }
}
```

---

## 注意点
- **深いコピー vs 浅いコピー**: 参照型のフィールドは深いコピーを考慮。
- **Unityでの考慮**: Unityでは、`Instantiate`を使用してGameObjectをクローンするのが一般的。
- **パフォーマンス**: 複雑なオブジェクトのクローンはパフォーマンスに影響する可能性。

---

## まとめ
プロトタイプパターンは、ゲーム開発でオブジェクトの効率的な複製が必要な場合に有用です。Unityでは、`Instantiate`と組み合わせることで、敵やアイテムのクローンを簡単に生成できます。