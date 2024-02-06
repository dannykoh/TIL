# IEnumerable

C#에서 `IEnumerable`은 `GetEnumerator()` 메소드 하나만을 정의하는 인터페이스로, `IEnumerator` 인터페이스를 반환합니다. 이를 통해 객체를 `foreach` 루프를 사용하여 반복할 수 있습니다. .NET Framework의 컬렉션 타입과 LINQ(Language Integrated Query)의 기본적인 구성 요소 중 하나로, 컬렉션의 요소에 순차적으로 접근할 수 있게 하면서도 컬렉션의 내부 구조를 노출하지 않습니다.

### Unity 게임 개발에서 IEnumerable의 최적 사용 사례

1. **컬렉션 반복:** 적, 수집품, 웨이포인트와 같은 아이템 컬렉션을 가지고 있을 때, 이를 반복하여 객체 활성화/비활성화, 효과 적용, 거리 계산 등의 작업을 수행합니다.

   ```csharp
   IEnumerable<GameObject> enemies = FindObjectsOfType<GameObject>().Where(go => go.CompareTag("Enemy"));
   foreach (var enemy in enemies) {
       // AI 활성화, 상태 변경 등의 작업 수행
   }
   ```

2. **LINQ 쿼리:** LINQ는 컬렉션에 대한 강력한 쿼리 기능을 제공합니다. 이는 특정 거리 내의 객체를 선택하거나 특정 속성을 가진 객체를 필터링, 정렬, 그룹화하는 데 매우 유용합니다.

   ```csharp
   var nearbyEnemies = enemies.Where(enemy => Vector3.Distance(player.transform.position, enemy.transform.position) < 10);
   foreach (var enemy in nearbyEnemies) {
       // 예를 들어, 적을 경고합니다.
   }
   ```

3. **커스텀 반복자:** `IEnumerable`을 구현하는 커스텀 컬렉션을 생성하여 특정 반복 로직을 제공할 수 있습니다. 예를 들어, 경로 탐색 시스템은 경로의 각 단계를 한 번에 하나씩 제공할 수 있습니다.

   ```csharp
   public class Path : IEnumerable<Vector3> {
       public IEnumerator<Vector3> GetEnumerator() {
           // 경로의 각 점을 순차적으로 반환하는 로직
       }

       IEnumerator IEnumerable.GetEnumerator() {
           return GetEnumerator();
       }
   }
   ```

`IEnumerable`을 사용하면 Unity 게임 개발에서 데이터 컬렉션을 효율적으로 관리하고, 다양한 게임 요소를 손쉽게 조작할 수 있는 강력한 도구를 갖추게 됩니다.

### 다른 Collection타입과의 차이점

`IEnumerable`은 C#에서 배열이나 `List<T>`와 같은 구체적인 컬렉션 타입이 제공하는 것과 유사한 기능을 제공하는 것처럼 보이지만, 추상화, 유연성 및 지연 평가 지원 측면에서 여러 가지 독특한 이점을 제공합니다:

1. **컬렉션 타입에 대한 추상화:** `IEnumerable`은 작업 중인 컬렉션의 타입을 추상화합니다. 이는 메서드가 특정 컬렉션 구현(예: 배열, 리스트, 세트 등)에 의존하지 않고도 컬렉션의 요소를 순회할 수 있게 해줍니다. 이로 인해 코드의 유연성이 증가하고, 다양한 타입의 컬렉션에 대해 같은 로직을 적용할 수 있습니다.

2. **유연성:** `IEnumerable` 인터페이스를 사용함으로써, 데이터를 저장하고 관리하는 방법에 대해 더 많은 유연성을 제공합니다. 예를 들어, 컬렉션의 실제 구현을 변경하더라도 `IEnumerable` 인터페이스를 사용하는 코드는 변경할 필요가 없습니다. 이는 유지 보수성과 코드의 재사용성을 향상시킵니다.

3. **지연 평가(Lazy Evaluation) 지원:** `IEnumerable`과 LINQ를 사용하면 컬렉션을 지연 평가할 수 있습니다. 즉, 실제로 필요할 때까지 요소의 처리를 지연시킬 수 있습니다. 이는 특히 대용량 데이터를 다룰 때 메모리 사용량을 줄이고 성능을 개선할 수 있는 장점을 제공합니다. 예를 들어, LINQ 쿼리는 실제로 결과가 필요할 때까지 실행되지 않습니다.

4. **컬렉션 크기의 유연성:** `IEnumerable`을 사용하면, 컬렉션의 크기가 고정되어 있지 않아도 됩니다. 예를 들어, 파일의 모든 줄을 읽거나 데이터베이스 쿼리 결과를 순회할 때, 전체 데이터 세트를 메모리에 로드할 필요 없이 요소를 한 번에 하나씩 처리할 수 있습니다.

이러한 이점들 덕분에, `IEnumerable`은 데이터 컬렉션을 다룰 때 높은 수준의 추상화와 유연성을 제공하며, 특히 대규모 데이터 세트나 다양한 컬렉션 타입을 사용하는 복잡한 애플리케이션 개발에 있어 매우 유용합니다.