# Evt Class
Global Event 로직의 기본 클래스

```cs
// No parameters
public class Evt
{
    private event Action _action = delegate { };

    public void Send()
    {
        _action.Invoke();
    }

    public void Register(Action listner)
    {
        _action -= listner;
        _action += listner;
    }

    public void Unregister(Action listner)
    {
        _action -= listner;
    }
}

// One parameter
public class Evt<T>
{
    private event Action<T> _action = delegate { };

    public void Send(T param)
    {
        _action.Invoke(param);
    }

    public void Register(Action<T> listner)
    {
        _action -= listner;
        _action += listner;
    }

    public void Unregister(Action<T> listner)
    {
        _action -= listner;
    }
}

// Two parameters
public class Evt<T1, T2>
{
    private event Action<T1, T2> _action = delegate { };

    public void Send(T1 param1, T2 param2)
    {
        _action.Invoke(param1, param2);
    }

    public void Register(Action<T1, T2> listner)
    {
        _action -= listner;
        _action += listner;
    }

    public void Unregister(Action<T1, T2> listner)
    {
        _action -= listner;
    }
}

// Three parametersㅇ
public class Evt<T1, T2, T3>
{
    private event Action<T1, T2, T3> _action = delegate { };

    public void Send(T1 param1, T2 param2, T3 param3)
    {
        _action.Invoke(param1, param2, param3);
    }

    public void Register(Action<T1, T2, T3> listner)
    {
        _action -= listner;
        _action += listner;
    }

    public void Unregister(Action<T1, T2, T3> listner)
    {
        _action -= listner;
    }
}

```

# Events Class
사용하고 싶은 이벤트를 정의

```cs
public static class Events
{
    public static readonly Evt param0 = new Evt();
    public static readonly Evt<T1> param1 = new Evt<T1>();
    public static readonly Evt<T1, T2> param2 = new Evt<T1, T2>();
    public static readonly Evt<T1, T2, T3> param3 = new Evt<T1, T2, T3>();
    // This can go on and on
}
```
