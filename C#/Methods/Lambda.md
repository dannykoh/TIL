# Lambda
C#에서 람다 함수는 익명 함수를 간결하게 정의할 수 있는 방법 중 하나. 람다 함수는 대개 더 간단한 형태의 함수를 만들 때 사용되며, 주로 델리게이트(delegate)나 함수형 프로그래밍 개념에서 자주 활용된다. 

### 일반적인 형식
```csharp
(parameter_list) => expression
```
여기서,
- 'parameter_list'는 람다 함수에 전달되는 매개변수(parameter)의 목록
- 'exression'은 람다 함수가 실행될 때 반환하는 결과를 계산하는 표현식

예를 들어, 간단한 정수 두 개를 더하는 람다 함수를 다음과 같이 정의할 수 있다.
```csharp
Func<int, int, int> add = (a, b) => a + b;
```
여기서 add 변수는 정수 두 개를 받아서 더한 결과를 반환하는 람다 함수를 나타내고, 이 함수를 호출하려면 다음과 같이 사용할 수 있다.
```csharp
int result = add(5, 3);
```
### 쓰임
람다 함수는 델리게이트나 LINQ 쿼리와 함께 사용되는 경우가 많다. 예를 들어, LINQ에서는 데이터 컬렉션을 쿼리하고 필터링하는 데 람다 함수를 자주 활용한다.

#### 델리게이트와 함께 사용되는 람다 함수
델리게이트는 메서드 시그니처를 나타내는 형식입니다. C#에서 델리게이트를 사용하면 함수를 변수에 할당하거나 매개변수로 전달할 수 있습니다. 람다 함수는 델리게이트와 함께 사용될 때 특히 유용합니다.
```csharp
Func<int, int, int> add = (a, b) => a + b;
Func<int, int, int> multiply = (x, y) => x * y;

int result1 = add(5, 3); // result1은 8입니다.
int result2 = multiply(4, 6); // result2는 24입니다.
```
위 예제에서 Func<int, int, int> 델리게이트를 사용하여 정수 두 개를 받아서 정수를 반환하는 람다 함수를 정의하고 호출한다.

#### LINQ 쿼리와 함께 사용되는 람다 함수
아래 예제에서 Where 메서드를 사용하여 람다 함수를 전달하여 리스트에서 짝수만 필터링하고 출력한다.
```csharp
using System;
using System.Linq;

class Program
{
    static void Main()
    {
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

        // LINQ 쿼리를 사용하여 짝수만 필터링합니다.
        var evenNumbers = numbers.Where(n => n % 2 == 0);

        // LINQ 쿼리 결과를 출력합니다.
        Console.WriteLine("짝수: ");
        foreach (var number in evenNumbers)
        {
            Console.WriteLine(number);
        }
    }
}
```