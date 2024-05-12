# Volatile

The volatile keyword indicates that a field might be modified by multiple threads that are executing at the same time. The compiler, the runtime system, and even hardware may rearrange reads and writes to memory locations for performance reasons. Fields that are declared volatile are excluded from certain kinds of optimizations. There is no guarantee of a single total ordering of volatile writes as seen from all threads of execution. For more information, see the Volatile class. [Link]("https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/volatile")

더 많은 정보는 링크에서 확인

### 선언하는 방법
```c#
public volatile int sharedStorage;
```

### volatile을 써야하는 case

```c#
public class Worker
{
    // This method is called when the thread is started.
    public void DoWork()
    {
        bool work = false;
        while (!_shouldStop)
        {
            work = !work; // simulate some work
        }
        Console.WriteLine("Worker thread: terminating gracefully.");
    }
    public void RequestStop()
    {
        _shouldStop = true;
    }
    // Keyword volatile is used as a hint to the compiler that this data
    // member is accessed by multiple threads.
    private volatile bool _shouldStop;
}

public class WorkerThreadExample
{
    public static void Main()
    {
        // Create the worker thread object. This does not start the thread.
        Worker workerObject = new Worker();
        Thread workerThread = new Thread(workerObject.DoWork);

        // Start the worker thread.
        workerThread.Start();
        Console.WriteLine("Main thread: starting worker thread...");

        // Loop until the worker thread activates.
        while (!workerThread.IsAlive)
            ;

        // Put the main thread to sleep for 500 milliseconds to
        // allow the worker thread to do some work.
        Thread.Sleep(500);

        // Request that the worker thread stop itself.
        workerObject.RequestStop();

        // Use the Thread.Join method to block the current thread
        // until the object's thread terminates.
        workerThread.Join();
        Console.WriteLine("Main thread: worker thread has terminated.");
    }
    // Sample output:
    // Main thread: starting worker thread...
    // Worker thread: terminating gracefully.
    // Main thread: worker thread has terminated.
}
```

With the volatile modifier added to the declaration of _shouldStop in place, you'll always get the same results (similar to the excerpt shown in the preceding code). However, without that modifier on the _shouldStop member, the behavior is unpredictable. 

The DoWork method may optimize the member access, resulting in reading stale data. Because of the nature of multi-threaded programming, the number of stale reads is unpredictable. Different runs of the program will produce somewhat different results. 컴파일러가 코드를 최적화하는 과정에서 예상치 못한 결과로 이어질 수 있기 때문에 해당 문제로 버그가 발생할 경우 관련 변수에 volatile 키워드를 붙여 문제를 예방하는 것이 좋음.