# Attributes

## [InitializeOnLoad] / [InitializeOnLoadMethod]

이 Attribute을 붙이면 유니티 프로젝트를 로드하거나 스크립트 리컴파일이 일어날 때 실행하게 해준다. 이벤트 콜백들을 미리 등록하거나 에디터 시작 후 바로 적용되어야하는 설정을 해줄때 유용하다.

| Usage | Description|
|---|--- |
| Static Contructor | Static class에 [InitializeOnLoad]를 불이면 해당 클래스의 생성자가 자동으로 실행되게 할 수 있다.  |
| Static Method|혹은 static 함수에 [InitializeOnLoadMethod]를 불이면 같은 조건에서 해당 함수가 실행되게 할 수 있다. |

### Execution Timing
에디터 시작 시 그 어떤 스크립트보다도 먼저 실행된다.

### Common Uses
- 커스턴 에디터 툴들의 초기화
- 프로젝트 설정 (Project Settings나 Preferences)
- 에디터 이벤트들의 콜백 등록
- 그 밖에 에디터 시작 시 자동으로 시작해야하는 자동화 툴

