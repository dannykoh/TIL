# An example code for Singleton system

1. Save the code below as Singleton.cs
2. Make a new class that you want to be a singleton class and make it inherit either from MonoBehaviourSingleton\<T> or MonoBehaviourSingletonPersistent\<T>

```csharp
using UnityEngine;

    public class MonoBehaviourSingleton<T> : MonoBehaviour where T : Component
    {
        private static T _instance;
        public static T Instance
        {
            get
            {
                if (_instance == null)
                {
                    var objs = FindObjectsOfType(typeof(T)) as T[];
                    if (objs.Length > 0)
                        _instance = objs[0];
                    if (objs.Length > 1)
                    {
                        Debug.LogError("There is more than one " + typeof(T).Name + " in the scene.");
                    }
                    if (_instance == null)
                    {
                        GameObject obj = new GameObject();
                        obj.hideFlags = HideFlags.HideAndDontSave;
                        _instance = obj.AddComponent<T>();
                    }
                }
                return _instance;
            }
        }
    }

    public class MonoBehaviourSingletonPersistent<T> : MonoBehaviour where T : Component
    {
        public static T Instance { get; private set; }

        public virtual void Awake()
        {
            if (Instance == null)
            {
                Instance = this as T;
                DontDestroyOnLoad(this);
            }
            else
            {
                Destroy(gameObject);
            }
        }
    }
```

## Example Implementation

```csharp
public class UIManager : MonoBehaviourSingleton<UIManager>
{
    ...
}
```
or
```csharp
public class UIManager : MonoBehaviourSingletonPersistent<UIManager>
{
    ...
}
```