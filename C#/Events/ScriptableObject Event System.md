# ScriptableObject를 이용한 이벤트 시스템
두 가지 스크립트 이용.
- GameEvent.cs
- GameEventListener.cs

## GameEvent.cs

각각의 이벤트의 리스너를 저장하고 Publish, Register/Unregister 이벤트 기능을 담당

```csharp
using System.Collections.Generic;
using UnityEngine;

[CreateAssetMenu(menuName = "GameEvent")]
public class GameEvent : ScriptableObject
{
    List<GameEventListener> listeners = new List<GameEventListener>();

    /// <summary>
    /// Send event to all listeners
    /// </summary>
    /// <param name="sender">The component that sends the event. Public variables/properties/methods can be accessed by the receivers of the events.</param>
    /// <param name="data">Generic-type data that can be sent along with the component. (For example: id)</param>
    public void Publish(Component sender, object data)
    {
#if UNITY_EDITOR
        PrintListeners(sender);
#endif

        for (int i = 0; i < listeners.Count; i++)
        {
            listeners[i].OnEventTriggered(sender, data);
        }
    }

    private void PrintListeners(Component sender)
    {
        string listenersName = string.Empty;
        for (int i = 0; i < listeners.Count; i++)
        {
            listenersName += $"<color=white>{listeners[i].name}</color> / ";
        }
        Debug.Log($"Global Game Event Fired <color=green>{name}</color>\n - Sender: <color=yellow>{sender.name}</color>  - Listeners: {listenersName}");
    }

    #region Manage Listeners

    public void Register(GameEventListener listener)
    {
        if (!listeners.Contains(listener))
        {
            listeners.Add(listener);
        }
    }

    public void Unregister(GameEventListener listener)
    {
        if (listeners.Contains(listener))
        {
            listeners.Remove(listener);
        }
    }

    #endregion
}

```

## GameEventListener

설정한 이벤트를 구독하고 필요한 Unity Action을 실행하는 컴포넌트.
제어하려는 오브젝트에 구독하려는 이벤트 숫자만큼 셋팅해줘야 함.

```csharp
using UnityEngine;
using UnityEngine.Events;

[System.Serializable]
public class CustomGameEvent : UnityEvent<Component, object> { }

/// <summary>
/// Attach this to the GameObject that needs to listen to the GameEvent(Scriptable Object) of interest.
/// </summary>
public class GameEventListener : MonoBehaviour
{
    public GameEvent gameEvent;

    public CustomGameEvent response;

    private void OnEnable()
    {
        gameEvent.Register(this);
    }

    private void OnDisable()
    {
        gameEvent.Unregister(this);
    }

    public void OnEventTriggered(Component sender, object data)
    {
#if UNITY_EDITOR
        if (data is null)
        {
            Debug.Log($"Global Game Event Received <color=green>{gameEvent.name}</color>\n - Listener: <color=white>{gameObject.name}</color>");
        }
        else
        {
            Debug.Log($"Global Game Event Received <color=green>{gameEvent.name}</color>\n - Listener: <color=white>{gameObject.name}</color> / Data: <color=orange>{data}</color>");
        }
#endif
        response.Invoke(sender, data);
    }
}

```