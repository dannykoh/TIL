# Nullable

```csharp
int? a = null;

if (a != null) // 혹은 if (a.HasValue). 같은 뜻.
{
    int b = a.Value; // 값을 a로 직접 가져올 순 없고 a.Value를 이용해야 함.
}
```

### 축약형 문법

```csharp
int b = number ?? 0;
```
위 문법은 number가 null이 아니면 number.Value를 넣어주고, null이면 기본값으로 0을 넣는다는 뜻이다.