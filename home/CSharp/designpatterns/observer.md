---
title: C# -observer-
description: C# -observer-
published: true
date: 2025-10-12T00:41:28.828Z
tags: c#, script, designpatterns, observer
editor: markdown
dateCreated: 2025-10-12T00:41:28.828Z
---

# オブザーバーパターン in C#

## 概要
オブザーバーパターンは、1対多の依存関係を定義する行動デザインパターンです。あるオブジェクト（サブジェクト）の状態が変化すると、関連するすべてのオブジェクト（オブザーバー）に通知します。ゲーム開発では、UI更新やイベント処理に適しています。

---

## オブザーバーパターンの特徴
- **1対多の関係**: サブジェクトが複数のオブザーバーに通知。
- **動的な登録/解除**: オブザーバーを動的に追加/削除可能。
- **疎結合**: サブジェクトとオブザーバーが独立して動作。

---

## 実装方法
C#では、イベントやデリゲートを使用してオブザーバーパターンを実装できます。

```csharp
using System;
using System.Collections.Generic;

// オブザーバーインターフェース
public interface IObserver
{
    void Update(string message);
}

// サブジェクト
public class Subject
{
    private List<IObserver> _observers = new List<IObserver>();
    private string _message;

    public string Message
    {
        get => _message;
        set
        {
            _message = value;
            Notify();
        }
    }

    public void Attach(IObserver observer) => _observers.Add(observer);
    public void Detach(IObserver observer) => _observers.Remove(observer);

    private void Notify()
    {
        foreach (var observer in _observers)
        {
            observer.Update(_message);
        }
    }
}

// 具体的なオブザーバー
public class ConcreteObserver : IObserver
{
    private string _name;

    public ConcreteObserver(string name)
    {
        _name = name;
    }

    public void Update(string message)
    {
        Console.WriteLine($"{_name} received message: {message}");
    }
}
```

### 使用例
```csharp
Subject subject = new Subject();
IObserver observer1 = new ConcreteObserver("Observer1");
IObserver observer2 = new ConcreteObserver("Observer2");

subject.Attach(observer1);
subject.Attach(observer2);

subject.Message = "Hello Observers!";
// 出力:
// Observer1 received message: Hello Observers!
// Observer2 received message: Hello Observers!
```

---

## メリット
1. **疎結合**: サブジェクトとオブザーバーが独立して動作。
2. **動的性**: オブザーバーの追加/削除が容易。
3. **再利用性**: 汎用的なイベント通知メカニズムとして使用可能。

## デメリット
1. **メモリリークのリスク**: オブザーバーの解除を忘れるとメモリリークが発生。
2. **パフォーマンス**: 多数のオブザーバーがいると通知に時間がかかる。

---

## ゲーム開発での使用例
ゲーム開発では、プレイヤーの状態変化（例：HPの減少）をUIや他のシステムに通知する際に有用です。

### 例: プレイヤーHPの監視（Unity向け）
```csharp
using UnityEngine;
using System;
using System.Collections.Generic;

public interface IHealthObserver
{
    void OnHealthChanged(int health);
}

public class Player : MonoBehaviour
{
    private List<IHealthObserver> _observers = new List<IHealthObserver>();
    private int _health = 100;

    public int Health
    {
        get => _health;
        set
        {
            _health = value;
            NotifyObservers();
        }
    }

    public void Attach(IHealthObserver observer) => _observers.Add(observer);
    public void Detach(IHealthObserver observer) => _observers.Remove(observer);

    private void NotifyObservers()
    {
        foreach (var observer in _observers)
        {
            observer.OnHealthChanged(_health);
        }
    }
}

public class HealthUI : MonoBehaviour, IHealthObserver
{
    public void OnHealthChanged(int health)
    {
        Debug.Log($"Health UI updated: HP = {health}");
    }
}

public class GameController : MonoBehaviour
{
    void Start()
    {
        Player player = gameObject.AddComponent<Player>();
        HealthUI healthUI = gameObject.AddComponent<HealthUI>();

        player.Attach(healthUI);
        player.Health -= 10; // 出力: Health UI updated: HP = 90
    }
}
```

---

## 注意点
- **メモリ管理**: オブザーバーの解除を忘れないよう注意。
- **Unityでの考慮**: Unityでは、UnityEventを活用することで簡潔に実装可能。
- **パフォーマンス**: 大量のオブザーバーによる通知のオーバーヘッドに注意。

---

## まとめ
オブザーバーパターンは、状態変化の通知に最適で、ゲーム開発ではUI更新やイベント処理に広く使用されます。C#のイベントシステムを活用することで、簡潔かつ効率的に実装できます。