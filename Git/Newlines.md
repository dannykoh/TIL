# Git 사용 시 NewLines 관리

Git을 형상관리툴로 사용 시 default 설정으로 NewLine 처리를 CRLF로 하는 경우가 많다. Windows에서 권장되는 NewLines 정책이 CRLF이기 때문. 

## CR, LF란?
CR, LF는 타자기에서 유래된 단어이다. 타자기로 문서를 작성할 때 한 줄의 텍스트를 다 입력했으면 아랫 줄로 입력부를 이동시켜줘야 한다. 아래줄로 이동하는 것이 Line Feed (LF)이고, 왼쪽 끝으로 밀어주는 것이 Carriage Return (CR)이다.

그럼 CRLF는? 복귀와 개행을 모두 포함하는 개념이다.

## OS별 줄바꿈처리

### Linux
기본값이 LF이다 (\n)

### Windows
기본값이 CRLF이다 (\r\n)

## LF를 사용해야하는 이유

LF를 사용해야하는 이유는 협업 때문인데, CRLF와 LF의 바이트코드가 다르기 때문에 git같은 형상관리툴에서 다른 코드로 인식함으로써 commit할 때 변경하지 않은 파일로 변경된 것으로 인식하게 된다. 그렇게 되면 여러개의 파일이 쓸데없이(?) 변경파일로 나열되기 때문에 보기 불편하다. 그래서 LF로 통일하는 것.

# 설정방법

## 1. Git 설정

### Global 설정

```cs
git config --global core.autocrlf input
```

### Repository별 설정
```cs
git config core.autocrlf input
```

## 2. .gitattribute 파일 변경
```
# Auto detect text files and perform LF normalization
# 여기가 포인트 
* text=auto eol=lf 

# C# and shader scripts should use LF line endings
*.cs text eol=lf
*.shader text eol=lf
*.cginc text eol=lf
*.compute text eol=lf
*.hlsl text eol=lf

# JSON and YAML files should use LF line endings
*.json text eol=lf
*.yaml text eol=lf
*.yml text eol=lf

# XML files should use LF line endings
*.xml text eol=lf

# Markdown files should use LF line endings
*.md text eol=lf

# Text files should use LF line endings
*.txt text eol=lf

# Unity-specific text asset files should use LF line endings
*.asmdef text eol=lf
*.asmref text eol=lf
*.unity text eol=lf
*.prefab text eol=lf
*.meta text eol=lf

# Binary files - Git should not alter these
*.png binary
*.jpg binary
*.jpeg binary
*.psd binary
*.fbx binary
*.tga binary
*.cubemap binary
*.anim binary
*.controller binary
*.mp3 binary
*.ogg binary
*.wav binary
*.aac binary
*.mp4 binary
*.unitypackage binary
*.exr binary
*.dll binary
*.exe binary

# Ignore logs, builds, and other non-source files
*.log text eol=lf
*.build binary

# Force text files in certain formats to have LF line endings
*.gradle text eol=lf
*.pro text eol=lf
*.properties text eol=lf

# Treat Visual Studio solution and project files as text
*.sln text eol=lf
*.csproj text eol=lf
*.userprefs text eol=lf

# Handle Unity serialized files
*.asset text eol=lf

```

## 3. Normalize

.gitattribute 파일을 수정했다면 모든 파일의 line endinds를 정규화해줘야 설정이 적용된다.
터미널을 열어 다음을 실행하자.

```
git add --renormalize .
git commit -m "Normalize all the line endings"
```