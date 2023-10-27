# How to Edit Script Template for Unity

Script templates are stored in %EDITOR_PATH%\Data\Resources\ScriptTemplates.

In this directory, below template files can be found:

```txt
81-C# Script-NewBehaviourScript.cs.txt
82-Javascript-NewBehaviourScript.js.txt
83-Shader__Standard Surface Shader-NewSurfaceShader.shader.txt
84-Shader__Unlit Shader-NewUnlitShader.shader.txt
...
```

If you want a different C# script template, edit the 81-C# Script-NewBehaviourScript.cs.txt file and leave the rest.

### Original
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class #SCRIPTNAME# : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #NOTRIM#
    }

    // Update is called once per frame
    void Update()
    {
        #NOTRIM#
    }
}
```

### After Revision
You can remove the redundant descriptive comments for Start and Update methods if you want to.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class #SCRIPTNAME# : MonoBehaviour
{
    void Start()
    {
        #NOTRIM#
    }

    void Update()
    {
        #NOTRIM#
    }
}
```