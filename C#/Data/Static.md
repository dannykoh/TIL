# Static
static 키워드는 변수나 함수, 클래스에 정적 속성을 부여하는 것으로 클래스로부터 객체를 생성하지 않고 변수나 함수를 호출할 수 있도록 해주는 것이다.

## Static variables (정적 변수)

```csharp
public class StaticTestClass
{
    public static int score;
}
```
정적 변수를 선언하기 위해서는 위의 예시 코드와 같이 static 키워드를 붙여서 변수를 정의하면 된다. 이렇게 선언한 정적 변수는 클래스로부터 객체를 생성하지 않아도 [클래스명.변수이름]의 형식으로 곧바로 사용할 수 있게 된다. 

public class MainClass

```csharp
public class MainClass
{
    public void Main()
    {
        StaticTestClass.score = 10;
    }
}
```

클래스의 일반 멤버 변수는 클래스의 객체가 생성될 때, 각 객체마다 따로 생기지만, 정적 변수는 해당 클래스가 처음으로 사용되는 때에 한 번만 초기화되어 계속 동일한 메모리를 사용하게 된다.

## Static Function (정적 함수)

```csharp
public class StaticTestClass
{
    public static int score;
    public int memberInt;

    public static void StaticFunction()
    {
        score = 10;  // Okay! static 변수는 호출할 수 있다.
        memberInt = 10;  // Error! static 함수 내에서 멤버변수는 호출할 수 없다.
    }
}

public class MainClass
{
    public void Main()
    {
        StaticTestClass.score = 10;
        StaticTestClass.StaticFunction();
    }
}
```
함수를 선언할 때, static 키워드를 붙여서 함수를 정의하면 정적 함수를 만들 수 있다. 이 정적 함수 역시 [클래스명.함수이름]의 형식으로 객체를 생성하지 않고 곧바로 호출할 수 있다.

단, 정적 함수는 객체가 생성되기 전에 호출이 가능하기 때문에, 정적 함수 내에서는 정적 변수가 아닌 일반 멤버 변수를 호출할 수 없다.

## Static class (정적 클래스)

```csharp
public static class StaticTestClass
{
    public static int score;

    static StaticTestClass()
    {
        score = 10;
    }

    public static void StaticFunction()
    {
        score = 20;
    }
}
```
정적 클래스는 모든 멤버가 정적 변수 혹은 정적 함수로 이루어진 것으로 객체를 생성할 수 없는 클래스이다. 모든 정적 멤버 변수 및 정적 멤버 함수는 [클래스명.변수이름] 혹은 [클래스명.함수이름]으로 호출된다.

정적 클래스는 정적 생성자를 가질 수 있는데 이 정적 생성자는 public, protected, private 등의 액세스 한정자를 사용할 수 없으며, 매개변수 역시 가질 수 없다.