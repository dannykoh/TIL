# Reflection
X-Ray와 같이 클래스와 같은 오브젝트 내부의 모든 정보를 조회 가능하게 해주는 기능.

### 개요
일반적으로 프로그래밍을 할때 다른 클래스의 기능과 속성을 사용하기위해 new로 인스턴스를 만들어 끌어다 쓰기를 한다.

#### Sample Code
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Temp
{
    public int tempValue;
    public int TempValue
    {
        get { return tempValue; }
        set
        {
            tempValue = value;
        }
    }
    public void Func()
    {
        Debug.Log("어떠한 기능");
    }
}

public class ReflectionTest : MonoBehaviour
{
    private void Start()
    {
        Temp temp = new Temp();

        Debug.Log("타입이름 : " + temp);
    }
}
```

### GetType을 사용한 코드 예시
(using System.Reflection; 선언을 하여 Reflection의 기능들을 사용할 수 있다.)

```csharp
public class ReflectionTest : MonoBehaviour
{
    private void Start()
    {
        Temp temp = new Temp(); 
        
        Debug.Log("타입이름 : " + temp.GetType().Name);
    }
    // GetType을 하여 클래스의 이름을 출력한 상태
    // 클래스의 정보만을 가져온다
}
```

### Reflection을 이용한 여러 Type 함수들

> **GetFields();** : 멤버 변수들을 가져옴.<br>
> **GetPropertys();** : 멤버 프로퍼티를 가져옴.<br>
> **GetMethods();** : 멤버 함수들을 가져옴

#### Sample Code
```csharp
using System; 				// System을 포함시켜준다
using System.Reflection; 	// System.Reflection 를 사용하겠다

public class Temp
{
    public float currentHP;
    public string name;

    public void Func()
    {
        Debug.Log("어떠한 기능");
    }
}

public class ReflectionTest : MonoBehaviour
{
    private void Start()
    {
        Temp temp = new Temp();

        // Type 이란 클래스는 테이터 타입의 정보가 담겨있는 클래스이다.
        Type type;

        type = temp.GetType();  // temp가 가지고있는 데이터타입의 정보를 넘겨준다
                                // type에 담기는건 temp의 클래스 정보이지 Temp 자체는 아니다.(Temp의 속성과 기능은 아직 못쓴다)
                                // 즉 가져온 것은 temp의 값이 아니라, 테이터 타입의 정보를 가져오는것이다 (Temp 데이터타입의 정보)
                                // 여기에는 currenthp, attackdamage 이런거 없어!!객체가 아님
                             
        temp.Func();
        // type.Func(); type에는 클래스 자체만을 넘겨주었기 때문에 Func()함수를 이용할 수 없다


        MethodInfo[] methods = type.GetMethods();
        foreach (MethodInfo method in methods)
        {
            Debug.Log("메소드의 이름 : " + method.Name);
        }// 배열로 이루어진 Temp클래스 안의 모든 함수들을 로그로 확인해본다


        FieldInfo[] fields = type.GetFields();
        foreach (FieldInfo field in fields)
        {
            Debug.Log("멤버 변수의 이름 : " + field.Name);
        }// 배열로 이루어진 Temp클래스 안의 모든 변수들을 로그로 확인해본다


        PropertyInfo[] properties = type.GetProperties();
        foreach (PropertyInfo property in properties)
        {
            Debug.Log("프로퍼티 이름 : " + property.Name);
        }// 배열로 이루어진 Temp클래스 안의 모든 변수들을 로그로 확인해본다
    }
}
```

### 그러면 생기는 물음

- 이런식으로 Get함수를 사용하여 원하는 클래스의 기능과 속성을 알 수 있다면?
- 그렇다면 Get함수들을 통해 Temp의 기능과 속성들을 가져와서 이용할수 있지 않을까? 
- 변수의 경우 GetField와 SetValue 를 통해 사용할 수 있다.
- 함수의 경우 GetMethod와 Invkoe 를 통해 사용할 수 있다.

## GetField, SetValue 를 통한 변수실행
GetField를 통해서 해당 클래스의 변수를 가져오는 방법

```csharp
using System;               // System을 포함시켜준다
using System.Reflection;    // System.Reflection를 사용하겠다

public class Temp
{
    public float currentHP;
    public float currentMana;
    public string name;
}

public class ReflectionTest : MonoBehaviour
{
    private void Start()
    {
        Temp temp = new Temp();
        Temp temp2 = new Temp();

        temp.currentHP = 100;
        temp2.currentHP = 300;

        Type type;
        type = temp.GetType();  

        
        type.GetField("currentHP").SetValue(temp, 400);
        type.GetField("name").SetValue(temp, "Test");
        type.GetField("currentMana").SetValue(temp, 30.5f);
        // SetValue의 매개변수 타입은 오브젝트로 받기 때문에 박싱을 통해들어가 어떤 데이터타입도 SetValue로 들어갈 수 있다
        // 이 과정에서 내부에서 리플렉션이 일어나고 있는것!! 왜냐하면 박싱과 언박싱을하기 위해서는 클래스에 대한 메타 정보를 알고 있어야 하기 때문.

        Debug.Log(type.GetField("currentHP").GetValue(temp));
        Debug.Log(type.GetField("currentHP").GetValue(temp2));
        Debug.Log(type.GetField("currentMana").GetValue(temp));
        // GetValue() 함수의 인자로 temp 를 안적어주면 GetValue 상태에서 어떤 녀석의 hp를 가져와줄지 모른다
        // 왜냐하면 GetField 만으로는 Class에대한 메타 정보만을 가져와 주기 떄문
    }
}
```

> 중요한점은 GetField만으로는 클래스의 객체에대한 정보를 가져오는것이 아닌 가져온 데이터타입의 메타정보만을 가져오게 된다 그러므로 type = temp.GetType(); 중 type 만으로는 Temp클래스에 만들어놓은 hp라던지 변수들을 불러오지 못한다 그러기에 GetValue(temp) 이런식으로 어떤 객체의 currentHP를 가져올것인지 말을 해줘야 한다.

>그러나 여기서 이상한점은 SetValue를 하는 부분에서 어떻게 데이터타입을 자동으로 알수 있는걸까? SetValue의 매개변수 타입들은 Object이기 때문에 박싱 과정을 통해 어떤 데이터 타입인지 적지 않아도 알아서 확인을 해준다 (이 과정에서도 리플렉션이 일어난다)

## GetMethod, Invoke 를 통한 함수실행

함수도 변수와 마찬가지로 GetMethod 를 통해 실행이 가능하다.

```csharp
using System;               // System을 포함시켜준다
using System.Reflection;    // System.Reflection를 사용하겠다

public class Temp
{
    public void Func()
    {
        Debug.Log("어떠한 기능");
    }
    public void FuncTest(int value)
    {
        Debug.Log("매개변수 를 추가한 기능, 매개변수 = " + value);
    }

    public int FuncReturnValue()
    {
        return 10;
    }

    protected void FuncProtected()
    {

    }
}

public class ReflectionTest : MonoBehaviour
{
    private void Start()
    {
        Temp temp = new Temp();
        Temp temp2 = new Temp();

        Type type;
        type = temp.GetType();  
        

        // 일반적 함수 호출
        type.GetMethod("Func").Invoke(temp, null);


        // 매개변수 값이 있는 함수 호출
        object[] args = new object[1];
        args[0] = 50;
        type.GetMethod("FuncTest")?.Invoke(temp, args);
        // Invoke 의 두번째 인자값은 오브젝트의 배열을 받기 때문에 이런식으로 활용이 가능하다


        // return 값이 있는 함수 호출
        var result = type.GetMethod("FuncReturnValue")?.Invoke(temp, null); // 오브젝트 형태로 리턴해주기 때문에
        Debug.Log((int)result);                                             // int로 형변환을 해준다
                                                                           
    }
}
```

## 요약
#### Reflection은 언제 사용해야 유용할까?
만약에 특정 클래스에 모든 함수들을 외부클래스에서 실행시켜야하는 경우가 있다면 기존에 하던거처럼 new로 인스턴스 만들어서 클래스안에 정의된 함수 하나하나 타입하는것보다 (이러면 그 클래스 함수 추가되거나 삭제될때마다 바꿔야함) 리플렉션 써서 Invoke 배열로 받아서 어떤 상황에서든 특정클래스 안의 함수 실행하게 하면 훨씬 효율적일 것이다.

> 추가적으로 아래와 같이 확장메소드, 제네릭, 리플렉션, 딥카피, 재귀함수를 사용하여 DeepCopy를 할 수도 있다.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
using System.Reflection;

public static class ExtensionMethod
{
    public static void AddComponentString(this GameObject value, string name)
    {
        value.AddComponent(Type.GetType(name));
    }

    public static T DeepCopy<T>(this T obj) where T : new()
    {
        Type type = obj.GetType();

        if(type.IsClass)
        {
            T clone = new T();
            FieldInfo[] fields = type.GetFields();
            foreach(FieldInfo field in fields)
            {
                if(field.FieldType.IsClass)
                {
                    field.SetValue(clone, (field.GetValue(obj)).DeepCopy());
                }
                else
                {
                    field.SetValue(clone, (field.GetValue(obj)));
                }
                return (T)clone;
            }
        }        
        return (T)obj;        
    }
}
```