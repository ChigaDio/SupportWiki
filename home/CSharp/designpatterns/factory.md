---
title: C# -Factory-
description: C# -Factory-
published: true
date: 2025-10-12T00:35:00.496Z
tags: c#, designpatterns, factory
editor: markdown
dateCreated: 2025-10-12T00:35:00.496Z
---

# ファクトリメソッドパターン in C#

## 概要
ファクトリメソッドパターンは、オブジェクトの生成をサブクラスに委譲する生成デザインパターンです。オブジェクト生成のロジックをカプセル化し、柔軟性と拡張性を向上させます。ゲーム開発では、異なる種類のオブジェクト（例：敵、アイテム）を動的に生成する際に役立ちます。

---

## ファクトリメソッドパターンの特徴
- **オブジェクト生成の分離**: オブジェクトの生成ロジックをクライアントコードから分離。
- **拡張性**: 新しいオブジェクトタイプを追加する際に、既存のコードを変更せずに済む。
- **サブクラスによる制御**: サブクラスがどのクラスをインスタンス化するかを決定。

---

## 実装方法
以下は、C#でのファクトリメソッドパターンの実装例です。

```csharp
// 製品のインターフェース
public interface IProduct
{
    void Use();
}

// 具体的な製品
public class ConcreteProductA : IProduct
{
    public void Use() => Console.WriteLine("Using Product A");
}

public class ConcreteProductB : IProduct
{
    public void Use() => Console.WriteLine("Using Product B");
}

// ファクトリ抽象クラス
public abstract class Creator
{
    // ファクトリメソッド
    public abstract IProduct CreateProduct();

    // 製品を使用するメソッド
    public void SomeOperation()
    {
        IProduct product = CreateProduct();
        product.Use();
    }
}

// 具体的なファクトリ
public class ConcreteCreatorA : Creator
{
    public override IProduct CreateProduct() => new ConcreteProductA();
}

public class ConcreteCreatorB : Creator
{
    public override IProduct CreateProduct() => new ConcreteProductB();
}
```

### 使用例
```csharp
Creator creator = new ConcreteCreatorA();
creator.SomeOperation(); // 出力: Using Product A

creator = new ConcreteCreatorB();
creator.SomeOperation(); // 出力: Using Product B
```

---

## メリット
1. **疎結合**: クライアントコードは具体的なクラスに依存せず、インターフェースに依存。
2. **拡張性**: 新しい製品を追加する際に、既存のコードを変更せずに新しいファクトリを追加可能。
3. **単一責任の原則**: オブジェクト生成ロジックを一箇所に集約。

## デメリット
1. **コードの複雑さ**: 簡単なオブジェクト生成にはオーバーヘッドになる場合がある。
2. **クラスの増加**: 製品ごとにファクトリクラスが必要になるため、クラス数が増える。

---

## ゲーム開発での使用例
ゲーム開発では、異なる種類の敵キャラを生成する際にファクトリメソッドパターンが有用です。

### 例: 敵生成ファクトリ（Unity向け）
```csharp
using UnityEngine;

// 敵のインターフェース
public interface IEnemy
{
    void Attack();
}

// 具体的な敵
public class Zombie : IEnemy
{
    public void Attack() => Debug.Log("Zombie attacks with bite!");
}

public class Skeleton : IEnemy
{
    public void Attack() => Debug.Log("Skeleton attacks with bow!");
}

// 抽象ファクトリ
public abstract class EnemyFactory : MonoBehaviour
{
    public abstract IEnemy CreateEnemy();
}

// 具体的なファクトリ
public class ZombieFactory : EnemyFactory
{
    public override IEnemy CreateEnemy() => new Zombie();
}

public class SkeletonFactory : EnemyFactory
{
    public override IEnemy CreateEnemy() => new Skeleton();
}

// 使用例
public class GameController : MonoBehaviour
{
    void Start()
    {
        EnemyFactory factory = gameObject.AddComponent<ZombieFactory>();
        IEnemy enemy = factory.CreateEnemy();
        enemy.Attack(); // 出力: Zombie attacks with bite!

        factory = gameObject.AddComponent<SkeletonFactory>();
        enemy = factory.CreateEnemy();
        enemy.Attack(); // 出力: Skeleton attacks with bow!
    }
}
```

---

## 注意点
- **インターフェースの設計**: 製品のインターフェースを適切に設計しないと、拡張性が損なわれる。
- **Unityでの考慮**: Unityでは、MonoBehaviourを継承するクラスは`new`で生成できないため、GameObjectにアタッチする形でファクトリを実装。
- **依存性の管理**: 依存性注入（DI）と組み合わせると、さらに柔軟性が増す。

---

## まとめ
ファクトリメソッドパターンは、ゲーム開発で異なる種類のオブジェクトを動的に生成する際に役立つパターンです。拡張性が高く、コードの再利用性や保守性を向上させますが、シンプルなケースではオーバーヘッドになる可能性があるため、適切な場面で使用しましょう。