# Generic 타입

## object

### 장점
- object는 int, float, string 등 모든 타입으로 저장할 수 있고, 불러올 때는 각 타입으로 캐스팅하여 사용할 수 있다.<br>
예시)
```csharp
object obj = 1;
object obj2 = 2.3f;
object obj3 = "Danny";

int i = (int)obj;
float j = (float)obj2;
string w = (string)obj3;
```
- List\<object>로 만들어 여러 가지 타입의 데이터를 한 리스트에 저장할 수도 있다.
```csharp
List<object> list = new List<object>;
list.Add(1);
list.Add(2.3f);
list.Add("Danny");
```
### 단점
- 명시적으로 타입을 지정하고 선언하는 데이터는 Stack영역에 저장되어 빠르지만, object는 복사 타입이 아닌 참조형이라 heap에 저장(boxing)된다. 따라서 해당 데이터를 조회할 때는 heap에 있는 값을 꺼내와(unboxing) stack에 저장하고 사용하는 작업을 해야하기 때문에 느리다. 

## T

Generic 타입을 다루고 싶은 클래스나 함수를 생성할 때 \<T>를 사용해주고 generic class 또는 generic method가 생성된다. 아래 예제를 살펴보자. 

### Generic class

```csharp
class Program
    {
        static void Main(string[] args)
        {
            MyList<int> intList = new MyList<int>();
            MyList<float> floatList = new MyList<float>();
            MyList<Monster> monsterList = new MyList<Monster>();
        }
    }

class Monster
{
    // Sample class
}

class MyList<T>
    {
        List<T> list = new List<T>();

        public T GetValue(int i)
        {
            return list[i];
        }

        public void Add(T item)
        {
            list.Add(item);
        }

        public void Remove(T item)
        {
            list.Remove(item);
        }
    }
```

Parameter가 더 많을 경우에는 아래와 같이 인자를 늘려주면 됨.
```csharp
class MyList<T, K, W>
{
    // Do Something      
}
```
필요한 경우 타입에 **조건**을 붙일 수 있는데,
```csharp
class MyList<T> where T : struct // T는 값 형식이어야 한다.
{
    // Do Something      
}
```
혹은
```csharp
class MyList<T> where T : class // T는 참조 형식이어야 한다.
{
    // Do Something      
}
```
혹은
```csharp
class MyList<T> where T : new() // 어떠한 인자도 받지 않는 기본 생성자가 있어야 한다.
{
    // Do Something      
}
```
혹은
```csharp
class MyList<T> where T : Monster // T는 Monster라는 부모 클래스를 상속받은 자식 클래스여야 한다.
{
    // Do Something      
}
```


### Generic method

```csharp
public void MyMethod<T>(T input)
{
    // Do Something
}
```
이렇게 정의하고 실제로 사용할 때는,
```csharp
static void Main(string[] args)
{
    MyMethod<int>(3);
    MyMethod<string>("Danny");
}
```
이렇게 사용한다.

