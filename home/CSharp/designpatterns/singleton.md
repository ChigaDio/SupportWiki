---
title: C# -Singleton-
description: C# -Singleton-
published: true
date: 2025-10-12T00:32:21.677Z
tags: c#, script, singleton, designpatterns
editor: markdown
dateCreated: 2025-10-12T00:32:21.677Z
---

# シングルトンパターン in C#

## 概要
シングルトンパターンは、デザインパターンの一つで、**クラスのインスタンスがアプリケーション全体で一つだけ存在する**ことを保証するパターンです。これにより、グローバルなアクセスポイントを提供し、特定のオブジェクトへの一貫したアクセスを可能にします。ゲーム開発やアプリケーション開発でよく使用されます。

---

## シングルトンパターンの特徴
- **インスタンスの単一性**: クラスが生成するインスタンスは一つだけ。
- **グローバルアクセス**: どこからでもそのインスタンスにアクセス可能。
- **遅延初期化**: 必要になるまでインスタンスを生成しない（オプション）。

---

## 実装方法
C#でのシングルトンの実装例を以下に示します。スレッドセーフな実装も含めています。

### 基本的なシングルトン実装
```csharp
public class Singleton
{
    // 唯一のインスタンスを保持する静的フィールド
    private static Singleton _instance;

    // インスタンスへの公開アクセスポイント
    public static Singleton Instance
    {
        get
        {
            // インスタンスがまだ作成されていなければ生成
            if (_instance == null)
            {
                _instance = new Singleton();
            }
            return _instance;
        }
    }

    // コンストラクタをprivateにすることで外部からのインスタンス化を防止
    private Singleton()
    {
        // 初期化処理
    }

    // 例: シングルトンで管理するメソッド
    public void DoSomething()
    {
        Console.WriteLine("Singleton is working!");
    }
}
```

### スレッドセーフなシングルトン実装
マルチスレッド環境では、上記の実装だと競合状態が発生する可能性があります。以下はスレッドセーフな実装例です。

```csharp
public class ThreadSafeSingleton
{
    // インスタンスを保持する静的フィールド（volatileでスレッドセーフを強化）
    private static volatile ThreadSafeSingleton _instance;
    private static readonly object _lock = new object();

    // 公開アクセスポイント
    public static ThreadSafeSingleton Instance
    {
        get
        {
            if (_instance == null)
            {
                lock (_lock)
                {
                    if (_instance == null)
                    {
                        _instance = new ThreadSafeSingleton();
                    }
                }
            }
            return _instance;
        }
    }

    // privateコンストラクタ
    private ThreadSafeSingleton()
    {
        // 初期化処理
    }

    public void DoSomething()
    {
        Console.WriteLine("Thread-safe Singleton is working!");
    }
}
```

### 静的初期化を使用したシンプルな実装
C#では、静的初期化を利用することで、より簡潔でスレッドセーフなシングルトンを実現できます。

```csharp
public class StaticSingleton
{
    // 静的フィールドでインスタンスを保持（.NETの静的初期化はスレッドセーフ）
    private static readonly StaticSingleton _instance = new StaticSingleton();

    // 公開アクセスポイント
    public static StaticSingleton Instance => _instance;

    // privateコンストラクタ
    private StaticSingleton()
    {
        // 初期化処理
    }

    public void DoSomething()
    {
        Console.WriteLine("Static Singleton is working!");
    }
}
```

---

## メリット
1. **単一インスタンスの保証**: アプリケーション全体で一つのインスタンスを共有できる。
2. **グローバルアクセス**: どこからでも簡単にアクセス可能。
3. **リソースの節約**: 不要なインスタンス生成を防ぎ、メモリを効率的に使用。
4. **遅延初期化**: 必要になるまでインスタンスを生成しない（基本実装やスレッドセーフ実装の場合）。

## デメリット
1. **グローバル状態の問題**: グローバルにアクセス可能なため、状態管理が複雑になり、バグの原因になる可能性。
2. **テストの難しさ**: シングルトンは依存関係を固定するため、単体テストが困難になる場合がある。
3. **スレッドセーフの複雑さ**: マルチスレッド環境では、適切なロック機構が必要。
4. **依存性の隠蔽**: シングルトンに依存するクラスが増えると、依存関係が不明瞭になる。

---

## ゲーム開発でのシングルトンの使用例
ゲーム開発では、シングルトンパターンは以下のような場面でよく使われます：
- **ゲームマネージャー**: ゲームの状態（スコア、レベル、進行状況など）を管理。
- **サウンドマネージャー**: BGMや効果音の再生を一元管理。
- **リソースマネージャー**: テクスチャやモデルなどのリソースを管理。

### 例: ゲームマネージャーの実装（Unity向け）
以下は、Unityで使用するゲームマネージャーのシングルトン実装例です。

```csharp
using UnityEngine;

public class GameManager : MonoBehaviour
{
    // 静的インスタンス
    private static GameManager _instance;

    public static GameManager Instance
    {
        get
        {
            if (_instance == null)
            {
                // シーン内にGameManagerが存在しない場合、新しく作成
                GameObject go = new GameObject("GameManager");
                _instance = go.AddComponent<GameManager>();
                DontDestroyOnLoad(go); // シーン遷移で破壊されないようにする
            }
            return _instance;
        }
    }

    // ゲームの状態
    public int Score { get; private set; }
    public bool IsGameOver { get; private set; }

    private void Awake()
    {
        // 既にインスタンスが存在する場合、このオブジェクトを破棄
        if (_instance != null && _instance != this)
        {
            Destroy(gameObject);
            return;
        }
        _instance = this;
        DontDestroyOnLoad(gameObject);
    }

    // スコアを追加するメソッド
    public void AddScore(int points)
    {
        Score += points;
        Debug.Log($"Score: {Score}");
    }

    // ゲームオーバー処理
    public void GameOver()
    {
        IsGameOver = true;
        Debug.Log("Game Over!");
    }
}
```

### 使用例
他のスクリプトから`GameManager`にアクセスする方法：

```csharp
public class Player : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Coin"))
        {
            GameManager.Instance.AddScore(10); // スコアを追加
            Destroy(other.gameObject);
        }
        else if (other.CompareTag("Enemy"))
        {
            GameManager.Instance.GameOver(); // ゲームオーバー
        }
    }
}
```

---

## 注意点
- **シングルトンの乱用を避ける**: シングルトンは便利ですが、過度に使用するとコードの保守性が低下します。依存性注入（DI）など、代替案も検討しましょう。
- **Unityでの注意**: Unityでは、`DontDestroyOnLoad`を使用してシーン遷移後もオブジェクトを保持することが一般的ですが、インスタンスの重複に注意。
- **テスト容易性を考慮**: シングルトンを使用する場合、モックやスタブを使ったテストが難しいため、インターフェースを活用するなどの工夫が必要。

---

## まとめ
シングルトンパターンは、C#やUnityでのゲーム開発において、単一のインスタンスを管理し、グローバルにアクセスする必要がある場合に非常に有用です。ただし、適切に使用しないとコードの保守性やテスト容易性が損なわれるため、注意が必要です。スレッドセーフな実装やUnity特有の考慮点を押さえて、適切な場面で活用しましょう！