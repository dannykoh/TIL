# Exception Handling

## Try...Catch...Finally

Game 쪽에선 잘 쓰이지 않긴 하지만 알아두자.

```csharp
namespace CSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                int a = 10;
                int b = 0;
                int c = a / b;
            }
            catch(DivideByZeroException e)
            {
                // if else 문법처럼 하나 해당되는걸 찾으면 실행 후 빠져나간다.
            }
            catch(Exception e)
            {
                // 모든 Exception들의 조상
            }
            finally
            {
                // 모든 exception 핸들링 후 마지막으로 무조건 실행되는 구문
            }
        }
    }
}
```

### Custom Exception
내가 직접 만든 Exception을 일부러 발생시킬 수도 있다.
```csharp
using System;

// Define a custom exception class
public class InsufficientFundsException : Exception
{
    public InsufficientFundsException()
    {
    }

    public InsufficientFundsException(string message)
        : base(message)
    {
    }

    public InsufficientFundsException(string message, Exception innerException)
        : base(message, innerException)
    {
    }
}

// Create a bank account class
public class BankAccount
{
    private decimal balance;

    public BankAccount(decimal initialBalance)
    {
        balance = initialBalance;
    }

    public void Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Amount must be greater than zero.", nameof(amount));
        }

        if (amount > balance)
        {
            // Throw a custom exception when there are insufficient funds
            throw new InsufficientFundsException("Insufficient funds to make the withdrawal.");
        }

        balance -= amount;
    }
}

public class Program
{
    public static void Main()
    {
        BankAccount account = new BankAccount(1000);

        try
        {
            // Try to withdraw an amount greater than the balance
            account.Withdraw(1500);
        }
        catch (InsufficientFundsException ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
            // You can also log the exception or perform other actions here
        }

        // Continue with other program logic
    }
}

```

### Error log 대신 Exception을 쓰는 이유는?

1. **Improved Readability and Code Structure**

    커스텀 익셉션으로 코드의 의도가 더 명확해진다. 로그를 조사해보기 전에 익셉션의 내용을 알 수 있기 때문에 가독성이 좋아지고 코드 관리가 수월해진다.

2. **Precise error handling**
    
    커스텀 익셉션으로 익셉션들마다 고유의 처리를 해줄 수 있다.. Generic한 성격의 log message보다 훨씬 디테일한 처리 가능. 

3. **Centralized error handling**

    커스텀 익셉션은 앱의 모든 단계에서 캐치하고 핸들링할 수 있기 때문에 중앙화되고 좀 더 일관적인 익셉션 처리를 할 수 있다. 익셉션 발생 시 재시도 작업, 로딩, 유저 메세지 출력 등의 처리도 가능하다.

4. **Custom error messages and data**

    커스텀 익셉션에 추가 메세지나 데이터를 붙일 수 있기 때문에 뭐가 잘못됐는지 더 많은 정보를 담을 수 있다.

5. **Forcing developers to handle errors**

    커스텀 익셉션을 작성함으로써 개발자들이 관련 에러를 무시하지 않고 반드시 관련된 처리를 하도록 강제할 수 있다. 이는 보다 좋은 퀄리티의 코드로 이어질 것이다.
