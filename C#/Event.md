# Event
- delegate와 유사하나 한번 더 래핑한 방식.<br>(delegate 함수는 아무때나 누구나 호출할 수 있기 때문에 매우 중요한 함수의 경우 위험할 수 있음.)
- Observer pattern. 이벤트를 구독한 함수를 한번에 호출.

## 예제

Program.cs
```csharp
namespace CSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            InputManager.InputKey += PrintChar;

            while (true)
            {
                InputManager.Update();
            }
        }

        static void PrintChar(char c)
        {
            Console.WriteLine($"{c} is pressed!");
        }
    }
}
```

InputManager.cs
```csharp
namespace CSharp
{
    // Observer Pattern
    public static class InputManager
    {
        public delegate void OnInputKey(char input); // 이벤트 형식 선언
        public static event OnInputKey? InputKey; // 구독자 모집

        public static void Update()
        {
            if (!Console.KeyAvailable)
                return;

            ConsoleKeyInfo keyInfo = Console.ReadKey();
            Console.WriteLine();
            InputKey?.Invoke(keyInfo.KeyChar);
        }
    }
}
```