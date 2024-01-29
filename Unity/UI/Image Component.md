# Unity Image Component

The Image component in Unity is used to display images or sprites on a UI canvas. It is commonly used for UI elements such as buttons, icons, or backgrounds.

## Adding an Image Component

To add an Image component to a UI element, follow these steps:

1. Select the UI element (e.g., a button) in the Unity editor.
2. In the Inspector window, click on "Add Component" and search for "Image".
3. Click on "Image" to add the component to the selected UI element.

## Configuring the Image Component

Once the Image component is added, you can configure its properties in the Inspector window. Some of the commonly used properties include:

- **Source Image**: Specifies the image or sprite to be displayed.
- **Color**: Sets the color of the image. You can adjust the alpha value to control transparency.
- **Preserve Aspect**: When enabled, the image will maintain its aspect ratio when its size changes.
- **Fill Method**: Determines how the image is filled within its RectTransform. Options include "Horizontal", "Vertical", "Radial 90", "Radial 180", and "Radial 360".
- **Fill Origin**: Specifies the starting point for filling the image.
- **Fill Amount**: Sets the amount of the image to be filled, ranging from 0 to 1.

## Using Sprites with the Image Component

To use a sprite with the Image component, follow these steps:

1. Import the sprite into your Unity project.
2. Assign the sprite to the "Source Image" property of the Image component.
3. Adjust the other properties as needed to achieve the desired visual effect.

## Useful Features

### Image.alphaHitTestMinimumThreshold
The alpha threshold specifies the minimum alpha a pixel must have for the event to considered a "hit" on the Image.

Alpha values less than the threshold will cause raycast events to pass through the Image. An value of 1 would cause only fully opaque pixels to register raycast events on the Image. The alpha tested is retrieved from the image sprite only, while the alpha of the Image Graphic.color is disregarded.

alphaHitTestMinimumThreshold defaults to 0; all raycast events inside the Image rectangle are considered a hit. In order for greater than 0 to values to work, the sprite used by the Image must have readable pixels. This can be achieved by enabling Read/Write enabled in the advanced Texture Import Settings for the sprite and disabling atlassing for the sprite.

```csharp
using UnityEngine;
using System.Collections;
using UnityEngine.UI; // Required when Using UI elements.

public class ExampleClass : MonoBehaviour
{
    public Image theButton;

    // Use this for initialization
    void Start()
    {
        theButton.alphaHitTestMinimumThreshold = 0.5f;
    }
}
```


## Conclusion

The Image component in Unity is a versatile tool for displaying images and sprites in UI elements. By configuring its properties and using sprites, you can create visually appealing and interactive user interfaces.

For more information, refer to the Unity documentation on the [Image Component](https://docs.unity3d.com/Manual/script-Image.html).
