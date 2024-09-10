# Singleton 클래스의 구현

## 1. 장점
- <b>전역접근 가능</b>: 싱글턴 패턴을 사용하여 리소스나 서비스의 전역 접근점을 만들 수 있다.
- <b>동시성 제어</b>: 공유 자원에 동시 접근을 제한하고자 사용할 수 있다.

## 2. 단점
- <b>유닛 테스트</b>: 싱글턴을 과도하게 사용하면 유닛 단위의 테스트가 어려워진다. 
- <b>잘못된 습관</b>: 싱글턴으로 어디서나 모든 것에 쉽게 접근하게 만들 수 있기 때문에, 코드 작성 시 보다 정교하게 접근하여 테스트하는 것이 귀찮게 느껴질 수 있다.

```csharp
using UnityEngine;

public class Singleton<T> : Monobehaviour where T : Component
{
    private static T _instance;

    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = FindObjectOfType<T>();

                if (_instance == null)
                {
                    GameObject obj = new GameObject();
                    obj.name = typeof(T).Name;
                    _instance = obj.AddComponent<T>();
                }
            }

            return _instance;
        }
    }

    public virtual void Awake()
    {
        if (_instance == null)
        {
            _instance = this as T;
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }
}
```