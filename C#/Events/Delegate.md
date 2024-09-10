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

### Sample Code

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

## Generic 타입의 delegate
인자나 리턴 타입을 지정하지 않고 generic으로 사용할 수도 있는데 형식은 다음과 같다.
```csharp
delegate Return MyFunc<T, Return>(T Item); // 어느 곳에서도 쓰일 수 있는 공용 delegate 
```
다만 위 예제는 하나의 인자만 받을 수 있는 단점이 있다. 그래서 인자의 갯수나 반환해야하는 값의 갯수만큼 공용 delegate을 준비하면 된다. 예를들면,
```csharp
delegate Return MyFunc<Return>();
delegate Return MyFunc<T, Return>(T item);
delegate Return MyFunc<T1, T2, Return>(T1 t1, T2 t2);
// 등등 조합은 무궁무진
```
그런데 다행히도 C#에서는 **Func**라는 이름으로 가능한 최대 조합이 미리 준비되어 있어 그냥 사용하면 된다.

## Func vs Action (pre-made delegate types in C#)
Func는 끝 인자에 out TResult가 존재하여 항상 리턴타입을 가져야 하는데, 리턴 타입이 void인것을 만들고 싶다면 그 때는 Action을 사용하면 된다.

### Func 사용 예제
```csharp
namespace CSharp
{
    enum ItemType
    {
        Weapon,
        Armor,
        Amulet,
        Ring
    }

    enum Rarity
    {
        Common,
        Uncommon,
        Rare,
        Epic
    }

    class Item
    {
        public ItemType ItemType;
        public Rarity Rarity;
    }

    class Program
    {
        static List<Item> Items = new List<Item>();

        delegate bool ItemSelector(Item item); // Item을 인자로 받아 판단해서 bool 값을 리턴해주는 delegate 형식

        static void Main(string[] args)
        {
            Items.Add(new Item { ItemType = ItemType.Weapon, Rarity = Rarity.Common });
            Items.Add(new Item { ItemType = ItemType.Armor, Rarity = Rarity.Rare });
            Items.Add(new Item { ItemType = ItemType.Amulet, Rarity = Rarity.Uncommon });
            Items.Add(new Item { ItemType = ItemType.Ring, Rarity = Rarity.Epic });

            // Anonymous Function (무명함수 혹은 익명함수)
            Item? weapon = FindItem(delegate (Item item) { return item.ItemType == ItemType.Weapon; });

            // Lambda Function
            Item? armor = FindItem((item) => { return item.ItemType == ItemType.Armor; });
            Item? ring = FindItem((item) => { return item.ItemType == ItemType.Ring; });

            // Delegate 저장해서 재사용
            Func<Item, bool> selector = new Func<Item, bool>((Item item) => { return item.ItemType == ItemType.Amulet; });
            Item? amulet = FindItem(selector);
        }

        static Item? FindItem(Func<Item, bool> selector)
        {
            foreach (var item in Items)
            {
                if (selector(item))
                    return item;
            }
            Console.WriteLine("Item not found.");
            return null;
        }
    }
}
```
### Action 사용 에제
```csharp
using System;

class Program
{
    // Action 델리게이트를 사용하여 반환 타입이 없는 메서드 참조를 정의한다.
    static Action<string> Greet = (name) =>
    {
        Console.WriteLine($"안녕하세요, {name}!");
    };

    static void Main()
    {
        // Action 델리게이트를 사용하여 메서드 호출
        Greet("Alice"); // "안녕하세요, Alice!" 출력
        Greet("Bob");   // "안녕하세요, Bob!" 출력
    }
}

```

### 정리
- delegate를 직접 선언하지 않아도 이미 만들어진 애들이 존재한다
    - 반환 타입이 있을 경우 **Func** 사용
    - 반환 타입이 없을 경우 **Action** 사용
