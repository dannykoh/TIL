# Singleton Pattern

## Sample Code

### Basic Implementation
```C#

using UnityEngine;

public class Singleton<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T _instance;
    private static object _lock = new object();

    public static T Instance
    {
        get
        {
            if (applicationIsQuitting)
            {
                Debug.LogWarning("[Singleton] Instance '" + typeof(T) +
                                 "' already destroyed on application quit." +
                                 " Won't create again - returning null.");
                return null;
            }

            lock (_lock)
            {
                if (_instance == null)
                {
                    _instance = (T)FindObjectOfType(typeof(T));

                    if (FindObjectsOfType(typeof(T)).Length > 1)
                    {
                        Debug.LogError("[Singleton] Something went wrong " +
                                       " - there should never be more than 1 singleton!" +
                                       " Reopenning the scene might fix it.");
                        return _instance;
                    }

                    if (_instance == null)
                    {
                        GameObject singleton = new GameObject();
                        _instance = singleton.AddComponent<T>();
                        singleton.name = "(singleton) " + typeof(T).ToString();

                        DontDestroyOnLoad(singleton);

                        Debug.Log("[Singleton] An instance of " + typeof(T) + 
                                  " is needed in the scene, so '" + singleton +
                                  "' was created with DontDestroyOnLoad.");
                    }
                    else
                    {
                        Debug.Log("[Singleton] Using instance already created: " +
                                  _instance.gameObject.name);
                    }
                }

                return _instance;
            }
        }
    }

    private static bool applicationIsQuitting = false;
    protected virtual void OnDestroy()
    {
        applicationIsQuitting = true;
    }
}

```

To use this singleton pattern for any Monobehavior class, simply derive your class from the Singleton<> base class like this:

```C#
public class GameManager : Singleton<GameManager>
{
    // Your GameManager code here
}

```

## UPs and DOWNs

The Singleton pattern is frequently used in Unity development, especially for managing game state, audio systems, game managers, and other unique components that should be instantiated once and remain available throughout the game. As with many design patterns, there are advantages and disadvantages associated with its use. Here's a breakdown of the ups and downs of using the Singleton pattern in Unity:

### UPs

**1. Global Access:** <br>Singletons provide global access to their instance, making it easy to communicate or access resources without passing references everywhere.

**2. Persist Across Scenes:** <br>Using Unity's DontDestroyOnLoad(), Singletons can easily persist across different game scenes, ensuring that data or functionality remains available.

**3. Lazy Initialization:** <br>Singletons can employ lazy initialization, meaning they are only instantiated when needed. This can help with resource management and performance.

**4. Control over Instantiation:** <br>Ensures that only one instance of the object exists in the game, which can prevent unexpected behaviors or duplicate data.

**5. Structured Code:** <br>When used correctly, Singletons can lead to cleaner, more organized code, as there's a clear understanding of where certain functionality is centralized.

### DOWNs

**1. Tight Coupling:** <br>Singleton pattern often results in tight coupling between classes. This can make it harder to change, test, or expand parts of your codebase.

**2. Complexity in Multithreaded Environments:** <br>In situations where multithreading is involved, ensuring that the Singleton remains thread-safe can add complexity.

**3. Misuse and Overuse:** <br>Because of its simplicity and the immediate solution it seems to provide, developers sometimes overuse the Singleton pattern. This can lead to spaghetti code where everything depends on a handful of Singletons, making the architecture hard to maintain or modify.

**4. Lifetime Management Issues:** <br>It can be challenging to determine when a Singleton should be destroyed or if it should persist for the entire lifetime of the application.

**5.Testing Challenges:** <br>Singletons can introduce challenges in unit testing. Since they maintain state and can't be easily reset, tests might influence one another.

**6. Potential Memory Usage:** <br>If not managed correctly, Singletons might occupy memory for the entirety of the application, even if they are not needed after a certain point.

**7. Hard Dependencies:** <br>Using Singletons can make it tempting to bypass the principles of dependency injection, leading to harder-to-test and more coupled code.

## Useful Tips
- Use Singletons judiciously. Just because you can make something a Singleton doesn't mean you should.

- For critical systems, consider using dependency injection or service locators to reduce tight coupling and improve testability.

- Regularly refactor and evaluate the necessity of a Singleton. As your game grows, its needs may change, and what started as a reasonable Singleton might no longer fit the bill.

## Useful Learning Material

* [Singletons in Unity (done right)](https://gamedevbeginner.com/singletons-in-unity-the-right-way/ "by John French @ December 23, 2021")