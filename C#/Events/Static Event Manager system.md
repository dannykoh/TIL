# 전역 이벤트 시스템
## 필수 스크립트
- GlobalEvent
- GlobalEventType
## 선택적 스크립트
클라이언트의 모듈 단위로 묶어서 GlobalEventType 클래스를 따로따로 만들어줘도 좋다.

## GlobalEvent.cs

```csharp
using System;

// No parameters
public class GlobalEvent
{
    private event Action _action = delegate { };

    public void Publish()
    {
        _action.Invoke();
    }

    public void Register(Action listener)
    {
        _action -= listener;
        _action += listener;
    }

    public void Unregister(Action listener)
    {
        _action -= listener;
    }
}

// One parameter
public class GlobalEvent<T>
{
    private event Action<T> _action = delegate { };

    public void Publish(T param)
    {
        _action.Invoke(param);
    }

    public void Register(Action<T> listener)
    {
        _action -= listener;
        _action += listener;
    }

    public void Unregister(Action<T> listener)
    {
        _action -= listener;
    }
}

// Two parameters
public class GlobalEvent<T1, T2>
{
    private event Action<T1, T2> _action = delegate { };

    public void Publish(T1 param1, T2 param2)
    {
        _action.Invoke(param1, param2);
    }

    public void Register(Action<T1, T2> listener)
    {
        _action -= listener;
        _action += listener;
    }

    public void Unregister(Action<T1, T2> listener)
    {
        _action -= listener;
    }
}

// Three parameters
public class GlobalEvent<T1, T2, T3>
{
    private event Action<T1, T2, T3> _action = delegate { };

    public void Publish(T1 param1, T2 param2, T3 param3)
    {
        _action.Invoke(param1, param2, param3);
    }

    public void Register(Action<T1, T2, T3> listener)
    {
        _action -= listener;
        _action += listener;
    }

    public void Unregister(Action<T1, T2, T3> listener)
    {
        _action -= listener;
    }
}
```

## GlobalEventType

아래와 같이 필요한 이벤트 추가

```csharp
using UnityEngine;

public class GlobalEventType
{
    public static GlobalEvent OnLogout = new(); // No parameters
    public static GlobalEvent<string> OnLogin = new(); // One parameter
    public static GlobalEvent<string, string> OnLoginWithId = new(); // Two parameters
    public static GlobalEvent<string, string, string> OnLoginWithIdAndPw = new(); // Three parameters
}

```

## 이벤트 부르는 쪽에서는

아래와 같이 이벤트를 발생시키고

```csharp
public class SomeClass
{
    void Start()
    {
        GlobalEventType.OnLogout.Publish();
    }
}
```

## 이벤트 구독 쪽에서는

아래와 같이 구독하면 됨.

```csharp
public class SomeClass
{
    void Start()
    {
        GlobalEventType.OnLogout.Register(HandleLogout);
    }
}
```

OnDestroy에서 구독취소하는 것을 잊지 말도록!
```csharp
public class SomeClass
{
    void Start()
    {
        GlobalEventType.OnLogout.Unregister(HandleLogout);
    }
}
```