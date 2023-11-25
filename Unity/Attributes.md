# Attributes

## 공통특징

- 클래스 또는 메소드 상단에 애트리뷰트를 명시한다.

- 컴포넌트로 넣지 않고, 스크립트로만 존재해도 실행된다.

- 정적 클래스나 상속에 관계 없이 동작한다.

- 메소드 애트리뷰트는 정적 메소드에만 동작한다.

## Editor Attribute

### [InitializeOnLoad] / [InitializeOnLoadMethod]

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

### [InitializeOnEnterPlayMode]

- using UnityEditor;
- 메소드 애트리뷰트
- 실행 타이밍 : 플레이 모드 진입(Awake() 호출 이전)

## Runtime Attribute

### [RuntimeInitializeOnLoadMethod]

- using UnityEngine
- 메소드 애트리뷰트
- 실행 타이밍 : 플레이 모드 진입(Awake(), OnEnable() 호출 이후)

## 실행 순서

1. [InitializeOnLoad] : 정적 생성자

2. [InitializeOnEnterPlayMode] : 정적 메소드

3. [InitializeOnLoadMethod] : 정적 메소드

4. Awake(), OnEnable() : 동일 클래스 내에서는 Awake() 우선

5. [RuntimeInitializeOnLoadMethod] : 정적 메소드

6. Start()