# Git
---
<br>



# 1. Git 이란 무엇인가
## Git의 핵심 기능
  ### 버전 관리
  - 문서나 코드의 이름을 수정하지 않고도 수정할 때마다 그 내용과 수정시간을 알 수 있다
  ### 백업
  - 고장의 위험이 있는 로컬 저장소 대신, 웹 클라우드에 원격 저장소를 만들어 자료를 백업해 놓을 수 있다
  ### 협업
  - 여러 사람이 원격 저장소의 파일을 내려받아 공동으로 작업이 가능하다.
  - 누가 어떤 부분을 수정했는지도 기록이 남기 때문에 오류 추적이 쉽다.
<br>

  #### ☝🏼 각 기능을 순서대로 알아가야 함
<br>


---
# 2. Git 사용법
## 2-0. CLI 사용
#### 윈도우에서 제공하는 CLI는 전부 사용 가능
- CMD
- PowerShell
- **Git Bash** ←
<br>


## 2-1. 깃 저장소 만들기
#### 1) 저장소를 만들고 싶은 경로로 진입한다.
#### 2) bash 명령줄에 "git init"을 입력
```shell
git init {dirname}
```
  - "git init" 뒤에 아무 것도 입력 하지 않으면 현재 경로를 Git 저장소로 만든다.
  - "git init" 뒤에 디렉토리 이름을 추가하면 현재 경로에서 그 이름을 가진 새로운 디렉토리가 추가되고, 그 디렉토리를 저장소로 만든다.
<br>

#### git 명령어 옵션 찾기
옵션을 찾고 싶은 명령어를 입력하고 뒤에 **--help** 입력
```shell
git {command} --help
```
<br>


## 2-2. 버전(Commit) 만들기
### 0) 버전 생성 단계
<br>

![.git_structure](https://i.imgur.com/AfM2om8.png)

  - **작업 트리**(Working Tree / Working Directory) : 사용자가 작업하고 있는 디렉토리
  - **스테이지**(Staging Area) : 버전으로 만들 파일이 대기하는 곳
  - **저장소**(Repository) : 스테이지에 대기하고 있던 파일들을 버전으로 만들어 저장하는 곳
<br>

### 1) 스테이지에 올리기(스테이징하기)
```shell
git add {path}     
# {path}와 일치하는 대상 스테이징 
# e.g. git add .

git add {pathspec} 
# {pathspec}과 동일한 종류의 파일 모두 스테이징 ex)
# e.g. git add *.py
```
<br>

### 2) 저장소에 버전 생성하기 (커밋하기)
```shell
git commit -m "messege" 
# 커밋과 함께 저장할 커밋 로그 저장
```
#### 스테이징과 커밋을 한번에 하기
```shell
git commit -am "messege" 
# untracked 상태가 아닌 모든 파일을 커밋(커밋 메시지 작성)
```
#### 방금 커밋한 메시지 수정하기
```shell
git commit --amend       
# 가장 최근의 커밋 메시지 수정 (기본 설정된 편집기가 실행됨)
```
<br>

#### ☝🏼 파일의 상태
<br>

![file_status](https://i.imgur.com/maihAno.png)

  - **Clean** : 작업 트리의 변경사항이 전혀 없음
  - **Untracked** : 한 번도 Git에서 버전 관리를 하지 않은 항목
  - **Unmodified** : 스테이징 되고 있지만, 수정 사항이 없는 항목
  - **Modified** : 수정된 항목
  - **Staged** : 스테이지에 올라간 항목
<br>



## 2-3. 커밋 관리하기

### 1) 조회하기
#### 스테이지 상태 확인하기
```shell
git status
```
<br>

#### 저장소에 저장된 버전 확인하기
```shell
git log           
# commit log 조회

git log --stat    
# commit log와 commit에 관련된 파일까지 조회

git log --oneline
# commit log를 한 줄씩 간략히 나타냄
```
<br>

#### ☝🏼 git log 결과화면
![git_log](https://i.imgur.com/bU6WV1q.png)

  - **최신 버전** : 최신 브랜치를 가리킴
  - **커밋 해쉬** : 커밋을 구별하는 ID
  - **커밋 로그** : 커밋 기록
<br>
#### 커밋의 변경 세부사항 확인하기
```shell
git diff {commit_hash_ID}
# 해당 ID의 커밋에서 어떤 파일이 변경되었는지 조회
```
<br>

### 2) 되돌리기
#### Modified → Unmodified
```shell
git checkout -- {path}  
# 수정한 상태인 path를 최근 버전의 상태로 초기화
```
#### Staged → Unstaged
```shell
git reset HEAD {path}  
# 스테이지에서 path 내리기
```

#### Commit → Unstaged
```shell
git reset HEAD^         
# 가장 최근 commit을 되돌리고, stage에서도 내림
```

#### Latest Commit → Old Commit
```shell
git reset {commit hash ID}  
# 해당 ID 버전으로 이동하고 그 이후의 commit은 모두 삭제됨
```
```shell
git revert {commit hash ID}
# 해당 ID 버전의 커밋을 취소하고, 그 바로 전 버전으로 이동함
# 취소 사유를 작성하는 메시지를 작성하는 편집기가 실행됨
```
<br>


---
# 3. Git의 Branch

## 3-0. 브랜치 개념

#### 브랜치
  - 저장소에서 공유되지 않는 부분, 저장소의 분기
<br>

#### 브랜치를 사용한 작업의 흐름
  - 브랜치 생성(branch) - 브랜치 이동(checkout) - 브랜치 병합(merge)
<br>


## 3-1. Branch 만들기
  - 기본 브랜치는 master
  - HEAD
    - 현재 브랜치를 가리키는 포인터
    - 현재 작업트리가 어떤 버전을 기반으로 작업 중인지 가리김
```shell
git branch {branch_name}
# {branch_name}을 가진 브랜치 생성

git branch
# 브랜치 정보 보기
```
<br>


## 3-2. Branch 이동 & 정보 확인
#### 브랜치 이동
 - 브랜치 이동을 통해 HEAD가 가리키는 브랜치를 변경할 수 있다.
```shell
git checkout {branch_name}
# 해당 브랜치로 이동하기

git checkout -b {branch_name}
# 브랜치를 생성하고, 해당 브랜치로 바로 이동하기
```

#### 브랜치 정보확인
```shell
git log --oneline
# 커밋 정보를 oneline으로 확인
# 현재 가리키고 있는 브랜치를 확인

git log --oneline --branches
# git log --oneline 에 더해
# 커밋마다 어떤 브랜치에서 만든 커밋인지 확인

git log --oneline --branches --graph
# git log --oneline --branches 에 더해
# 분기 정보를 그래프 형태로 표시
```

#### ☝🏼 --oneline 옵션을 빼거나 --stat 옵션을 적용해도 됨
<br>

#### 브랜치 간 차이점 확인
```shell
git log {branch_name1}..{branch_name2}
# branch_name1과 branch_name2 사이의 차이점이 무엇인지 확인
# 커밋 단위로 알려줌
```
<br>


## 3-3. Branch 병합하기

  - 현재 브랜치와 다른 브랜치를 병합할 수 있다.
```shell
git merge {branch_name}
# 현재 checkout하고 있는 branch와 branch_name을 병합
# 커밋 메시지를 작성하여야 함
```

#### ☝🏼 Fast Forward 병합
  - master 브랜치와 병합하였을 때 master 브랜치에 아무 변화가 없다면, master 의 포인터가 병합할 브랜치의 최신 커밋을 가리키는 동작만 수행하는데, 이를 Fast-Forward merge라고 부름
  - Fast-Forward 병합 시, 따로 커밋 메시지를 작성하지 않음
<br>


## 3-4. 파일 충돌

  - 브랜치 병합 시, 같은 파일의 수정 사항이 겹칠 때 충돌이 발생
  - git의 기본 시스템은 충돌을 일으키고 있는 파일에 어느 부분이 충돌을 일으키는 지 텍스트로 표시해준다.
  - 
#### 충돌 발생 해결 방법
##### 1. 충돌이 발생하고 있는 파일을 사용자가 직접 수정한다.
##### 2. 해결한 파일을 커밋한다.
<br>

## 3-5. 브랜치 관리하기
 - 병합이 끝난 브랜치를 숨기거나 삭제할 수 있다.
#### 숨기기
```shell
git branch -d {branch_name}
# 브랜치 삭제
# 나중에 같은 이름의 브랜치를 만들면 예전 작업 내용이 그대로 나타남
```

#### 되돌리기
브랜치가 여러개일 때 버전을 되돌리면, 돌아간 버전 이후의 커밋 된 내용은 모두 사라짐
```shell
git reset {commit_hash_ID}
```
<br>


---
# 4. Git의 Stash
### Stash
  - 현재 작업 중인 파일들을 다른 곳에 따로 저장하는 기능
  - 파일을 수정하고 커밋하지 않은 상태에서 급하게 다른 파일을 커밋해야 할 경우, 현재 아직 커밋하지 않고 작업 중인 파일을 잠시 다른 곳에 감춰둘 수 있음
  - stashing은 아직 커밋되지 않은, tracked 상태인 파일들에 대해서만 동작함
<br>

## 4-1. Stashing
#### Stash에 올리기
```shell
git stash
# 현재 작업 트리 상태를 stash에 저장함
```
#### Stash의 내용을 다시 가져오기
```shell
git stash pop
# stash에 저장된 내용을 작업 트리로 가져옴
```
#### Stash 충돌
  - stash pop을 할 때, stash에 있는 내용과 stashing 중에 작업한 내용이 서로 충돌을 일으킬 수 있음
  - 이 경우 일반 파일 충돌의 경우와 동일하게 해결하면 된다 (수정 → 커밋)

#### ☝🏼 스테이징된 파일의 경우, stashing & pop을 한 뒤 스테이지에서 내려와 있음