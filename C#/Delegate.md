# Delegate (대리자)

## 정의
- 대리자를 통해 함수 자체를 인자로 넘겨줄 수 있는 기능. 
- UI 로직에서 유용하게 쓰일 수 있는데, UI로직과 Game로직이 ButtonClicked()라는 함수 안에 섞여 있으면 좋지 않으므로 delegate을 사용해 분리해줄 수 있다.
```csharp
static void ButtonClicked(/* Some Function */)
{
    // 인자로 넘겨 받은 함수를 호출
}
```

## 형식 (format)

```csharp
delegate int OnClicked();
```
- delegate 타입 : 함수 자체를 인자로 넘겨주는 타입
- return 타입 : int
- 입력 타입 : void
- OnClicked : 이 delegate 형식의 이름.

그래서 위 **ButtonClicked()** 함수에 적용을 해보면,
```csharp
delegate int OnClicked();

static void ButtonClicked(OnClicked clickedFunction)
{
    clickedFunction();
}
```
이렇게 사용할 수 있다.

## Sample Code

```csharp
namespace CSharp
{
    class Program
    {
        delegate int OnClicked(); // delegate 형식 선언

        static void Main(string[] args)
        {
            ButtonPressed(TestDelegate); // delegate 호출
            Console.Read();
        }

        // 인자로 OnClicked 대리자 형식의 함수를 받을 함수
        static void ButtonPressed(OnClicked onClicked) 
        {
            onClicked.Invoke();
        }

        // 인자로 넘겨 줄 함수. delegate 형식과 동일해야 함.
        static int TestDelegate() 
        {
            Console.WriteLine("Hello Delegate!");
            return 0;
        }
    }
}
```