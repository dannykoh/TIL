# Abstract과 Virtual의 차이

In C#, both `abstract` and `virtual` are keywords used to define methods in classes, but they serve different purposes:

## Abstract

### Abstract Methods:
   - An abstract method is declared using the `abstract` keyword in an abstract class. An abstract class is a class that cannot be instantiated on its own and is meant to be subclassed.
   - An abstract method does not have an implementation in the abstract class; it only provides a method signature (name, parameters, and return type).
   - Subclasses (derived classes) of an abstract class must provide an actual implementation (method body) for all abstract methods declared in the base abstract class. Failure to do so will result in a compilation error.
   - Abstract methods are used when you want to define a contract that derived classes must adhere to, ensuring that certain methods are implemented in all derived classes but allowing each derived class to provide its own specific implementation.

Here's an example of an abstract method in an abstract class:

```csharp
public abstract class Shape
{
    public abstract double CalculateArea();
}
```

## Virtual
### Virtual Methods:
   - A virtual method is declared using the `virtual` keyword in a class (not necessarily abstract). It provides a default implementation in the base class but can be overridden in derived classes.
   - Subclasses have the option to override a virtual method using the `override` keyword, providing their own implementation.
   - Virtual methods are used when you want to provide a default behavior in the base class but allow derived classes to customize or extend that behavior by providing their own implementations.

Here's an example of a virtual method in a non-abstract class:

```csharp
public class Vehicle
{
    public virtual void StartEngine()
    {
        Console.WriteLine("Engine started.");
    }
}

public class Car : Vehicle
{
    public override void StartEngine()
    {
        base.StartEngine();
        Console.WriteLine("Car engine started.");
    }
}
```

In this example, `StartEngine` is a virtual method in the `Vehicle` class, and the `Car` class overrides it to provide its own behavior while still invoking the base class behavior using `base.StartEngine()`.

In summary, `abstract` methods are meant to define a contract that derived classes must implement, while `virtual` methods provide a default implementation in the base class that can be optionally overridden in derived classes.