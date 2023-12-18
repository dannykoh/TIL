# GitAttribute

## 개요
디렉토리와 파일별로 다른 git 설정을 적용하는 기능. gitattribute 파일은 아무 위치에 둘 수 있지만 보통은 프로젝트의 최상단 폴더에 둔다.(.gitignore과 마찬가지로.) 혹, 이 파일을 커밋하고 싶지 않으면 .git/info/attributes로 파일을 만든다.

## 기능
이 Attribute로 Merge는 어떻게 할지, 텍스트가 아닌 파일은 어떻게 Diff 할지, checkin/checkout 할 때 어떻게 필터링할지 정해줄 수 있다.

## Binary 파일
기본적으로 git은 어떤 파일이 바이너리 파일인지 알지 못하기 때문에 gitattribute에 알려줘야한다. 그리고 텍스트 파일 중에서도 diff 할 수 없는, 바이너리 파일과 유사한 파일 타입이 있는데 이러한 파일도 git에 바이너리라고 알려줘야 한다. 반면, 바이너리 파일임에도 diff가 가능한 파일타입도 있다.

## Binary 파일로 설정해야 하는 텍스트 파일
사실 텍스트 파일이지만 만든 목적과 의도를 보면 바이너리 파일인 것이 있다. 예를 들어 Mac의 Xcode는 .pbxproj 파일을 만든다. 이 파일은 IDE 설정 등을 디스크에 저장하는 파일로 JSON 포맷이다. 모든 것이 ASCII인 텍스트 파일이지만 실제로는 간단한 데이터베이스이기 때문에 텍스트 파일처럼 취급할 수 없다. 그래서 여러 명이 이 파일을 동시에 수정하고 Merge 할 때 diff가 도움이 안 된다. 이 파일은 프로그램이 읽고 쓰는 파일이기 때문에 바이너리 파일처럼 취급하는 것이 옳다.

### 어떻게 설정하면 될까?
모든 pbxproj 파일을 바이너리로 파일로 취급하는 설정은 아래와 같다. .gitattributes 파일에 넣으면 된다.
```
*.pbxproj binary
```
이제 pbxproj 파일은 CRLF 변환이 적용되지 않는다. git show 나 git diff 같은 명령을 실행할 때도 통계를 계산하거나 diff를 출력하지 않는다.

## Binary 파일을 diff하기
Git은 바이너리 파일도 Diff 할 수 있다. Git Attribute를 통해 Git이 바이너리 파일을 텍스트 포맷으로 변환하고 그 결과를 diff 명령으로 비교하도록 하는 것이다.

### Use Case
먼저 이 기술을 인류에게 알려진 가장 귀찮은 문제 중 하나인 Word 문서를 버전 관리하는 상황을 살펴보자. Git 저장소에 넣고 이따금 커밋하는 것만으로도 Word 문서의 버전을 관리할 수 있다. 그렇지만 git diff 를 실행하면 아래와 같은 메시지를 볼 수 있을 뿐이다.
```
$ git diff
diff --git a/chapter1.docx b/chapter1.docx
index 88839c4..4afcb7c 100644
Binary files a/chapter1.docx and b/chapter1.docx differ
```
직접 파일을 하나하나 까보지 않으면 두 버전이 뭐가 다른지 알 수 없다. Git Attribute를 사용하면 이를 더 좋게 개선할 수 있다. .gitattributes 파일에 아래와 같은 내용을 추가한다.
```
*.docx diff=word
```
이건 *.docx 파일의 두 버전이 무엇이 다른지 Diff 할 때 “word” 필터를 사용하라고 설정하는 것이다. 그럼 “word” 필터는 뭘까? 이 “word” 필터도 정의해야 한다. Word 문서에서 사람이 읽을 수 있는 텍스트를 추출해주는 docx2txt 프로그램을 사용하여 Diff에 이용한다.

## Merge 전략
파일마다 다른 merge 전략을 사용하게 설정할 수 있다. 가령 merge할 때 충돌이 날 것 같은 파일이 있다고 쳤을 때, gitattribute로 이 파일은 항상 내 코드를 사용하게 설정할 수 있다.
가령, 이 설정은 다양한 환경에서 운영하려고 만든 환경 브랜치를 merge할 때 좋다. 예로 브랜치에 database.xml이라는 DB 설정파일이 있는데 이 파일은 브랜치 마다 달라서 이 파일은 merge하면 안된다. 이 경우 attribute를 다음과 같이 설정하면 이 파일은 두고 merge한다.
```
database.xml merge=ours
```
그리고 ours merge 전략을 다음과 같이 정의한다.
```
$ git config --global merge.ours.driver true
```
다른 브랜치로 이동해서 Merge를 실행했을 때 database.xml 파일에 대해 충돌이 발생하는 대신 아래와 같은 메시지를 보게 된다.
```
$ git merge topic
Auto-merging database.xml
Merge made by recursive.
```
Merge 했지만 ```database.xml``` 파일은 원래 가지고 있던 파일 그대로다.