The concept of "Extension Methods" in C# is quite interesting and useful, especially when you're working with existing classes or libraries and you want to add some new methods without altering their source code.

### What are Extension Methods?

1. **Definition**: Extension methods are special static methods that allow you to "extend" an existing type (class, interface, or struct) with new methods, without modifying the original type.
2. **How They Work**: These methods are defined in static classes but are called as if they were instance methods on the extended type.
3. **Purpose**: They're particularly useful for adding methods to types that you don't own, like classes in the .NET Framework or third-party libraries.

### Basic Rules for Extension Methods

- They must be declared in a static class.
- They must be static methods.
- The first parameter specifies the type that the method operates on, and it is preceded by the `this` keyword.

### Examples of Extension Methods

Let's consider you're working with strings and you often need to check if a string is a valid email address. Normally, the `String` class doesn't provide this method. Here's how you can create an extension method for this:

1. **Creating the Static Class**:
   ```csharp
   public static class StringExtensions
   {
   }
   ```

2. **Adding an Extension Method**:
   ```csharp
   public static class StringExtensions
   {
       public static bool IsValidEmail(this string str)
       {
           // A simple email validation logic (for illustration)
           return str.Contains("@") && str.Contains(".");
       }
   }
   ```

3. **Using the Extension Method**:
   Now, you can use the `IsValidEmail` method on any string object as if it were a built-in method:
   ```csharp
   string email = "example@email.com";
   bool isValid = email.IsValidEmail(); // This will use the extension method
   ```

### Why Use Extension Methods?

- **Enhancing Functionality**: They allow you to add methods to types without creating a new derived type.
- **Readability**: They can make your code more readable and maintainable.
- **Flexibility**: Useful for adding methods to classes that you cannot modify, such as sealed classes or classes in third-party libraries.

In summary, extension methods in C# provide a flexible and powerful way to add functionality to existing types, enhancing the expressiveness and capabilities of your code without modifying the original types.

## Some Useful Unity C# Extension Methods

### 1. Converting from Vector3 to Vector3Int

```cs
public static Vector3Int ConvertToVector3(this Vector3 vec3)
{
   return new Vector3Int((int)vec3.x, (int)vec3.y, (int)vec3.z);
}
```
### 2. Resetting the Transform
```cs
public static void ResetTransformation(this Transform trans)
{
   trans.position = Vector3.zero;
   trans.localRotation = Quaternion.identity;
   trans.localScale = new Vector3(1, 1, 1);
}
```
### 3. Rotating a 2D Vector
```cs
public static Vector2 Rotate(this Vector2 vector, float degrees)
{
   float sin = Mathf.Sin(degrees * Mathf.Deg2Rad);
   float cos = Mathf.Cos(degrees * Mathf.Deg2Rad);

   float tx = vector.x;
   float ty = vector.y;
   vector.x = (cos * tx) - (sin * ty);
   vector.y = (sin * tx) + (cos * ty);
   return vector;
}
```
### 4. Returning a normalized degree value
```cs
public static float RotationNormalizedDeg(this float rotation)
{
   rotation = rotation % 360f;
   if (rotation < 0)
       rotation += 360f;
   return rotation;
}
```
### 5. Setting children layer value recursively
```cs
public static void SetLayerRecursively(this GameObject gameObject, int layer)
{
   gameObject.layer = layer;
   foreach (Transform t in gameObject.transform)
        t.gameObject.SetLayerRecursively(layer);
}
```
### 6. Checks if GameObject has a certain component
```cs
public static bool HasComponent(this Component component) where T : Component
{
   return component.GetComponent() != null;
}
```
### 7. Destroys all children recursively of a GameObject
```cs
public static void DestroyChildren(this GameObject parent)
{
   Transform[] children = new Transform[parent.transform.childCount];
   for (int i = 0; i < parent.transform.childCount; i++)
        children[i] = parent.transform.GetChild(i);
   for (int i = 0; i < children.Length; i++)
        GameObject.Destroy(children[i].gameObject);
}
```
### 8. Children of one GameObject gets transferred to another GameObject
```cs
public static void MoveChildren(this GameObject from, GameObject to)
{
    Transform[] children = new Transform[from.transform.childCount];
    for (int i = 0; i < from.transform.childCount; i++)
         children[i] = from.transform.GetChild(i);
    for (int i = 0; i < children.Length; i++)
         children[i].SetParent(to.transform);
}
```
### 9. Checks if Renderer can be seen by camera
```cs
public static bool IsVisibleFrom(this Renderer renderer, Camera camera)
{
   var planes = GeometryUtility.CalculateFrustumPlanes(camera);
   return GeometryUtility.TestPlanesAABB(planes, renderer.bounds);
}
```
### 10. Checks if a Rect component intersects another Rect
```cs
public static bool Intersects(this Rect source, Rect rect)
{
   return !((source.x > rect.xMax) || (source.xMax < rect.x) || (source.y > rect.yMax) || (source.yMax < rect.y));
} 
```