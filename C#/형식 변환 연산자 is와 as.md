# C# 형식 변환 연산자 is와 as

C#은 형변환을 위해 is와 as, 두 개를 제공합니다.

## Summary

| <div style="width:70px">연산자</div> | 설명 |
| :----: | :---- |
| is |	객체가 해당 형식에 해당하는지를 검사하여 그 결과를 bool 값으로 반환 |
| as |	형식 변환 연산자와 같은 역할을 함. 다만 형변환 연산자가 변환에 실패하는 경우 예외를 던지는 반면에 as 연산자는 객체 참조를 null로 만듦.|

### is 연산자
객체 item 형식이 HealingPosion 형식인지 검사하여 bool 값으로 반환하는 예제입니다. <br>
item 객체가 HealingPosion형으로 선언되었으므로 Main 메소드 안의 if문은 정상적으로 실행됩니다.



```csharp
class Item
{
    public void Using()
    {
        Console.WriteLine("Uinsg()");
    }
}

class HealingPosion : Item
{
    public void Heal()
    {
        Console.WriteLine("Heal()");
    }
}

class MainApp
{
    static void Main(string[] args)
    {
        Item item = new HealingPosion();
        HealingPosion posion;

        if (item is HealingPosion)
        {
            posion = (HealingPosion)item;
            posion.Heal();
        }
    }
}

```

### as 연산자
Main 메소드에서 as 연산자가 사용되었습니다. 객체 item이 HealingPosion 형식이라면 객체 posion에 대입하고 아니라면 null을 대입합니다.

```csharp
class Item
{
    public void Using()
    {
        Console.WriteLine("Uinsg()");
    }
    }

class HealingPosion : Item
{
    public void Heal()
    {
        Console.WriteLine("Heal()");
    }
}

class MainApp
{
    static void Main(string[] args)
    {
        Item item = new HealingPosion();

        HealingPosion posion = item as HealingPosion;

        if(posion != null)
        {
            posion.Heal();
        }
    }
}
```