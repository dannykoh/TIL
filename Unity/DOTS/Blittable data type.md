# Blittable Data Type

In the .NET development environment, a blittable data type is a type that can be efficiently and directly represented in memory, without any need for data conversion or marshaling when interacting with unmanaged code or performing low-level memory operations. Blittable data types are often used when working with platform invoke (P/Invoke), interop, or when dealing with memory buffers.

### Here's a simpler breakdown:

1. **Efficient Memory Representation:** <br>Blittable data types are designed to be stored in memory in a straightforward way, without any extra processing or transformation.

2. **Interop-Friendly:** <br>They are suitable for easily exchanging data between managed .NET code and unmanaged code, like C or C++.

3. **No Conversion Needed:** <br>When you use blittable data types, you don't have to perform any special conversions or translations when moving data between managed and unmanaged code. This makes your code faster and more efficient.

Common examples of blittable data types in .NET include simple numeric types like _**int, float, double, and structs**_ composed of blittable types. Complex objects, like class instances, may not be blittable because they often contain additional metadata and may require special handling when crossing the managed/unmanaged boundary.

## Unity Learn description ([Link](https://learn.unity.com/tutorial/part-2-data-design?uv=2022.3&courseId=60132919edbc2a56f9d439c3#639af14aedbc2a7364da206f))

Traditional OOP techniques (such as working with GameObjects and MonoBehaviours) confine the majority of user code to the main thread, which means you only get to use a fraction of the potential processing power of most modern CPUs. However, multithreaded code typically takes longer to write, because of the amount of time you need to spend debugging subtle problems like race conditions.

The **Unity job system** makes it much easier to write multithreaded code without race conditions because it contains a safety system which effectively bans code that would allow race conditions to occur. To be able to check for potential race conditions efficiently, the job system introduces a constraint: you can only create jobs that operate on blittable data types. 

**Blittable data** is data that can be copied into memory from disk or from another part of memory, in a single block copy operation, with no need to fix broken data references once it has been copied. Blittable data types include int, float, bool, and structs that contain only those types. Data types which live on the managed heap and involve references to memory locations, such as classes, strings and managed generic containers are not blittable. You should design your data to use only blittable types.

Another major advantage of using blittable data is that the code that transforms it can be compiled by Burst, which results in highly-optimized native code. You can apply the Burst compiler to all jobs and methods that you write in High-Performance C# (HPC#), a subset of C# .NET which excludes non-blittable managed data types. For more information about what data types, keywords, and language features are compatible with HPC#, see the C# Language Support pages of the Burst User Guide.

Familiarize yourself with what constitutes Burst compatible code so that you can Burst compile as many of your jobs as possible.