# Most-Used Audio Features in Unity

### Components
- AudioSource - 소리 발생의 근원지
- AudioClip - 재생할 소리
- AudiListener - 듣는 귀

### 작동 원리
- 소리를 발생시킬 오브젝트에 `Audio Source` 컴포넌트를 붙여주고, 이 컴포넌트에 원하는 `Audio Clip`을 할당하면 된다.
- Volume: 소리 크기
- Pitch: 재생 속도

## Sound Manager
게임오브젝트가 비활성화 혹은 파괴되면 오브젝트에 붙어있는 Audio Source 가 재생하던 소리들까지 전부 중단된다. 재생기가 사라졌기 때문이다! 위와같이 클립이 끝까지 재생될 때까지 재생시간 동안 대기한 후에 파괴하게할 수도 있지만 이는 좋은 방법이 아니다. 그래서 경우에 따라 오브젝트에 종속되지 않게끔 오디오 클립을 재생해야 한다. 그래서 사운드 매니저가 필요하다!

## Play()
AudioClip을 재생한다. 같은 AudioClip을 설정하고 Play를 연달아 실행할 경우 해당 클립이 새로 재생되는 것처럼 들리게 된다.

초 단위의 딜레이를 주고 싶은 경우 `AudioSource.PlayDelayed`를 사용한다.

클립 재생 시점을 보다 정확하게 제어하기 위해서는 `AudioSource.PlayScheduled`를 이용하도록 한다.

## PlayOneShot(AudioClip clip)
동일하게 AudioClip을 재생하는 함수이나, Play()와의 차이점은 이미 재생되고 있는 Clip을 취소하지 않는다는 것이다. 보통 총격발 소리 같이 반복적으로 재생되는 소리에 알맞는 함수이다.

## PlayClipAtPoint(AudioClip clip, Vector3 position)
World space 상 Vector3로 지정한 위치에서 Audio Clip을 재생한다. 재생이 완료되면 자동으로 파괴되는 Audio Source를 셀프 생성한다.