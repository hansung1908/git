# git
- 흔히 말하는 tree 구조로 저장되며 폴더에는 blob이라는 파일을 가짐
- 이때 새롭게 커밋을 실행하면 이전 것을 복사해서 새로 만드는 것이 아닌 원래 폴더 안에 폴더를 생성하여 변경 내용을 저장
- 각 폴더에는 고유한 해시코드를 들고 있어서 이를 통해 복구 및 조작이 가능

### 깃헙에 커밋 추가까지의 흐름
```shell
# 깃헙 내용 복사 후 연결, 깃헙 저장소를 미리 만든 경우
git clone 깃헙 주소

# 변경 내용 감지
git init

# 변경 내용 추가
# .은 변경 내용 전체를 뜻함
git add .

# 메세지와 함께 버전을 만들어 기록
git commit -m '커밋 메세지'

# 깃허브 내용 내쪽으로 갱신
git pull origin main

# 커밋된 내용을 깃허브에 저장
git push origin main
```
- 위 내용을 모두 진행하면 깃허브에 변경 내용이 하나의 버전으로 올라감

### 깃 설정
```shell
# 깃 이름을 hansung1908로 설정
git config --global user.name "hansung1908"

# 깃 이메일을 hansung1908@naver.com으로 설정
git config --global user.email "hansung1908@naver.com"
```

### 깃 상태
```shell
# 깃 헤더 위치, 커밋 여부, 변경 내용 저장 여부 등 상태를 보여줌
git status

# 여태 커밋한 내용을 시간순으로 나열
git log
```

### 깃 복구
```shell
# 커밋 로그만 변경, 변경 내용 추가 여부와 원본 내용은 그대로
git reset --soft 커밋 해쉬 코드 4자리

# 커밋 로그가 바뀌고 변경 내용이 추가되지 않은 상태, 원본 내용은 그대로
git reset --mixed 커밋 해쉬 코드 4자리

# 커밋 로그와 원본 내용을 변경, 이전 커밋에 파일이 없다면 해당 파일은 삭제
git reset --hard 커밋 해쉬 코드 4자리

# 여태 커밋한 로그를 변경 및 삭제없이 그대로 보여줌
git reflog
```

### 깃 커밋 변경
```shell
# 커밋된 메세지 내용을 변경
git commit --amend -m "커밋 메세지"

# 위와 동일
git reset --soft 이전 커밋 해쉬코드 4자리
git commit -m "커밋메세지"

# 헤더를 기준으로 자신을 포함한 이전 3개의 커밋을 통합
# 기준은 변경하는 내용중 가장 오래전에 만들어진 커밋
git rebase -i head~3
vi 에디터로 이동
i 입력
변경할 로그 pick을 s로 수정
esc
:q
```

### branch
- '가지'라는 뜻으로 원래 버전을 다루던 main branch에서 모종의 이유로 다른 개발을 해야할 경우 분기점을 두어서 main과 다른 branch를 두어 개발

```shell
# 해당 헤더 위치를 기준으로 분기점을 두어 새로운 브랜치 생성
git branch 브랜치 이름

# 해당 브랜치로 커밋되도록 헤더를 변경
git checkout 브랜치 이름

# 브랜치 생성과 동시에 헤더 이동
git checkout -b 브랜치 이름

# 헤더가 가리키고 있는 브랜치로 해당 브랜치를 합침
# 메인 브랜치에 별다른 변경이 없다면 fast forward merge
# 변경이 있다면 3 way merge
git merge 브랜치 이름
```
- 같은 파일을 변경하여 머지 충돌시 합쳐진 내용을 대표가 처리해서 다시 커밋해야 함

### 깃헙 연결
```shell
# 깃헙과 파일을 연결
git remote add origin 깃헙 프로젝트 주소

# 깃헙 연결 주소 확인
git ls-remote

# 메인 브랜치에 파일 업로드
git push origin main

# 깃헙에 있는 파일 다운로드
git pull origin main

# init + remote + pull
git clone 깃헙 프로젝트 주소
```

### 커밋 티켓팅
- 커밋 메세지에 이슈 번호를 명시
```text
git commit -m "커밋 메세지입니다. #1"
```

- 이슈 번호를 통해
  - 커밋 메세지 요약내용, 실제 자세한 내용은 이슈를 통해 확인
  - 이슈에 포함된 사람들을 확인 가능 (리뷰어까지)
  - 이슈를 해결하며 논의 했던 내용들을 확인 가능

##### 워크 플로우
- 특정 프로젝트에 대해 일을 부여받음 (새 기능 개발, 버그 픽스 등)
- -> GitHub나 사내 GitLab의 이슈 기능에 새로운 Issue(티켓) 생성
- -> 해당 프로젝트에 관련된 커밋 작성시 번호 기재

### git stash

- 작업 중인 변경 사항을 임시로 저장하고, 작업 디렉토리를 깨끗한 상태로 되돌릴 수 있는 Git 명령어입니다.
- 다른 브랜치로 전환하거나, 작업을 잠시 멈추고 다른 작업을 해야 할 때 유용하게 사용됩니다.

##### 주요 명령어

```bash
# stash 저장:
git stash

# 또는 메시지를 남길 경우:
git stash save "메시지"

# stash 목록 확인:
git stash list

# stash 적용:

# 최근 stash 적용:
git stash apply

# 특정 stash 적용:
git stash apply stash@{번호}

# stash 적용 후 삭제:
git stash pop

# stash 삭제:
git stash drop stash@{번호}

# 모든 stash 삭제:
git stash clear
```

특징 및 주의사항

- 스택 구조: 여러 번 stash할 수 있고, 가장 최근에 넣은 stash가 먼저 꺼내집니다.
- 추적되지 않은 파일(untracked): 기본적으로 stash에 포함되지 않지만, -u 옵션을 추가하면 함께 저장할 수 있습니다.
- 브랜치에 종속되지 않음: stash는 브랜치에 종속되지 않으므로, 다른 브랜치에서도 복원할 수 있습니다(충돌 주의).
- 스태시 후 추가 작업: stash 후 추가 작업을 하고 stash를 적용하면, stash에 저장된 내용이 현재 작업 트리에 덧씌워집니다. 충돌이 발생할 수 있으니, 충돌이 있다면 직접 해결(수동 머지)해야 하며, 자동으로 깔끔하게 merge되지 않습니다.
