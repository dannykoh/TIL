# Struct

In C#, structs are value types that are primarily used for lightweight objects that have a small memory footprint and can be efficiently copied. The most common use cases for structs include:

1. **Small Data Structures:** <br>Structs are ideal for representing small, simple data structures that contain a few related fields. Examples include Point, Rectangle, Color, and DateTime.

```csharp
struct Point
{
    public int X;
    public int Y;
}
```

2. **Performance-Critical Code:** <br>Structs can be more efficient than classes in certain scenarios, especially when dealing with large arrays of data or frequently passing objects by value. Because structs are stored on the stack, they can be faster to allocate and deallocate compared to objects stored on the heap (like classes).

3. **Immutable Types:** <br>Structs are naturally immutable, which means their values cannot be changed after they are created. This immutability is useful for representing values that should not be modified, such as currency amounts, measurements, or coordinates.

```csharp
public struct Temperature
{
    private readonly double valueInCelsius;

    public Temperature(double celsius)
    {
        valueInCelsius = celsius;
    }

    public double Celsius => valueInCelsius;
    public double Fahrenheit => (valueInCelsius * 9 / 5) + 32;
}
```

4. **Avoiding Heap Allocations:** <br>Structs are allocated on the stack, so they help reduce heap memory allocations. This can be beneficial in situations where you want to minimize garbage collection overhead or memory usage, such as in game development or real-time systems.

5. **Interop with Unmanaged Code:** <br>Structs are often used when working with unmanaged code or external libraries, as they can be used to define the layout of data structures that need to be passed to or received from native code.

It's important to note that structs have limitations compared to classes. They should be used when the benefits of their value-type semantics and performance optimizations outweigh their limitations, such as the lack of inheritance and the inability to have default constructors. Careful consideration of your application's specific needs and performance requirements is essential when deciding whether to use a struct or a class.