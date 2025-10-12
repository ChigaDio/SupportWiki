---
title: C# -builder-
description: C# -builder-
published: true
date: 2025-10-12T00:37:32.717Z
tags: c#, script, designpatterns, builder
editor: markdown
dateCreated: 2025-10-12T00:37:32.717Z
---

# ビルダーパターン in C#

## 概要
ビルダーパターンは、複雑なオブジェクトを段階的に構築するための生成デザインパターンです。オブジェクトの構築プロセスをカプセル化し、異なる構成のオブジェクトを柔軟に生成できます。ゲーム開発では、複雑なキャラクターやアイテムの生成に適しています。

---

## ビルダーパターンの特徴
- **段階的構築**: オブジェクトをステップごとに構築。
- **柔軟性**: 同じ構築プロセスで異なるオブジェクトを生成可能。
- **可読性**: 複雑なオブジェクトの構築プロセスを明確に記述。

---

## 実装方法
以下は、C#でのビルダーパターンの実装例です。

```csharp
// 製品クラス
public class Product
{
    public string PartA { get; set; }
    public string PartB { get; set; }
    public string PartC { get; set; }

    public void Display()
    {
        Console.WriteLine($"Product: PartA={PartA}, PartB={PartB}, PartC={PartC}");
    }
}

// ビルダーインターフェース
public interface IBuilder
{
    void BuildPartA();
    void BuildPartB();
    void BuildPartC();
    Product GetResult();
}

// 具体的なビルダー
public class ConcreteBuilder : IBuilder
{
    private Product _product = new Product();

    public void BuildPartA() => _product.PartA = "Part A";
    public void BuildPartB() => _product.PartB = "Part B";
    public void BuildPartC() => _product.PartC = "Part C";
    public Product GetResult() => _product;
}

// ディレクター
public class Director
{
    public void Construct(IBuilder builder)
    {
        builder.BuildPartA();
        builder.BuildPartB();
        builder.BuildPartC();
    }
}
```

### 使用例
```csharp
Director director = new Director();
IBuilder builder = new ConcreteBuilder();
director.Construct(builder);
Product product = builder.GetResult();
product.Display(); // 出力: Product: PartA=Part A, PartB=Part B, PartC=Part C
```

---

## メリット
1. **複雑な構築の簡略化**: 複雑なオブジェクトを段階的に構築。
2. **再利用性**: 同じビルダーで異なる構成のオブジェクトを生成可能。
3. **可読性**: 構築プロセスが明確で、コードが読みやすい。

## デメリット
1. **コード量の増加**: ビルダークラスやディレクターが必要なため、コード量が増える。
2. **単純なオブジェクトには不向き**: 簡単なオブジェクトにはオーバーヘッド。

---

## ゲーム開発での使用例
ゲーム開発では、キャラクターや武器などの複雑なオブジェクトを構築する際に有用です。

### 例: キャラクター生成ビルダー（Unity向け）
```csharp
using UnityEngine;

public class Character
{
    public string Name { get; set; }
    public int Health { get; set; }
    public string Weapon { get; set; }

    public void Display()
    {
        Debug.Log($"Character: Name={Name}, Health={Health}, Weapon={Weapon}");
    }
}

public interface ICharacterBuilder
{
    void SetName();
    void SetHealth();
    void SetWeapon();
    Character GetResult();
}

public class WarriorBuilder : ICharacterBuilder
{
    private Character _character = new Character();

    public void SetName() => _character.Name = "Warrior";
    public void SetHealth() => _character.Health = 100;
    public void SetWeapon() => _character.Weapon = "Sword";
    public Character GetResult() => _character;
}

public class MageBuilder : ICharacterBuilder
{
    private Character _character = new Character();

    public void SetName() => _character.Name = "Mage";
    public void SetHealth() => _character.Health = 50;
    public void SetWeapon() => _character.Weapon = "Staff";
    public Character GetResult() => _character;
}

public class CharacterDirector
{
    public void Construct(ICharacterBuilder builder)
    {
        builder.SetName();
        builder.SetHealth();
        builder.SetWeapon();
    }
}

public class GameController : MonoBehaviour
{
    void Start()
    {
        CharacterDirector director = new CharacterDirector();

        ICharacterBuilder warriorBuilder = new WarriorBuilder();
        director.Construct(warriorBuilder);
        Character warrior = warriorBuilder.GetResult();
        warrior.Display(); // 出力: Character: Name=Warrior, Health=100, Weapon=Sword

        ICharacterBuilder mageBuilder = new MageBuilder();
        director.Construct(mageBuilder);
        Character mage = mageBuilder.GetResult();
        mage.Display(); // 出力: Character: Name=Mage, Health=50, Weapon=Staff
    }
}
```

---

## 注意点
- **ビルダーの設計**: ビルダーのインターフェースを適切に設計しないと、柔軟性が損なわれる。
- **Unityでの考慮**: MonoBehaviourを使用する場合、GameObjectの生成やコンポーネントの追加を考慮。
- **ディレクターの役割**: ディレクターはオプションであり、クライアントが直接ビルダーを操作する場合も。

---

## まとめ
ビルダーパターンは、複雑なオブジェクトの構築を段階的に行う際に有用です。ゲーム開発では、キャラクターやアイテムの生成に適しており、柔軟性と可読性を高めます。