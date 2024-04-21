# 깃 최초 설정 
## 깃 전역 설정으로 사용자 이름과 이메일 설정하기 
- 깃 계정과 별개로 컴퓨터에 사용자 이름과 이메일 주소 설정 필요 
- 다른 사람과 협업할 때 작업 내역과 작업자를 표시하는데 활용  

```console
git config --global user.name "사용자 이름"
git config --global user.email "사용자 이메일"
```

## 사용자 이름 설정 확인 
```console
git config --global user.name
```

## 사용자 이메일 설정 확인 
```console
git config --global user.email
```

## 기본 브랜치 이름을 main으로 변경하기 
- 기본 브랜치 이름을 main이나 trunk로 변경하기 
- 기존 사용중인 master는 master/slave 같은 용어로 썼는데 인종차별을 연상시켜 다른 용어로 대체
```console
git config --global init.defaultBranch main
```

<br>

# 프로젝트 생성 및 관리 
## .git 폴더 생성하기 
- 깃이 관리하는 프로젝트 내역들이 .git 폴더에 들어감 
- .git 폴더를 삭제하면 깃 관리 내역이 모두 사라짐
- 맥 숨김파일 보기 (Cmd + Shift + .)
```console
git init
```

## 폴더 상태 보기 
- 현재 폴더의 상황을 깃의 관점으로 보여줌 
```console
git status
```

## 깃에서 파일 무시하기 
- .gitignore 파일 생성 
- 생성된 파일에 깃으로부터 격리하고 깃이 해당 파일을 무시하도록 지정 
- 예시
    - 특정 파일 이름을 그대로 입력하면 깃이 해당 파일을 무시 
        - file.c    #모든 file.c 무시
    - 파일 이름 앞에 슬래시를 입력하면 해당 파일이 있는 최상위 폴더의 파일만 무시 
        - /file.c   #최상위 폴더의 file.c
    - 특정 확장자의 파일을 무시하고 싶을때 별표와 확장자 입력 
        - *.c  
    - 무시하도록 지정된 파일 중에서 예외처리는 파일 이름 앞 느낌표 붙이기  
        - !not_ignore_this.c #.c확장자이지만 무시하지 않을 파일 
    - 폴더와 하위 폴더 그리고 파일까지 무시하기 - 확장자 없이 이름만 작성 
        - logs #logs라는 이름의 파일 또는 폴더와 그 내용들 
    - 해당 폴더와 그 안의 내용 무시 - 이름 끝에 슬래시 붙이기 
        - logs/ #logs란 이름의 폴더와 그 내용들 
    - 특정한 폴더의 특정한 파일 - 폴더와 파일이름, 확장자 앞에 *붙이면 해당하는 폴더 안 모든 확장자 
        - logs/debug.log 
        - logs/*.c  #logs 폴더 안의 debug.log와 .c 파일들 
    - 특정 폴더의 모든 하위 폴더의 특정 확장자 무시하기 
        - logs/**/*.c #logs 폴더 하위에 있는 모든 폴더의 .c 확장자 파일 
## 파일의 변화를 버전에 담기 
- 스테이지에 올리기 
```console
git add 파일명 
```

## 프로젝트에서 일어난 모든 변화를 버전에 담기 
- 스테이지에 올리기 
```console
git add . 
```

## 버전 커밋하기 
- 커밋 메시지가 한 줄 일때 
```console
git commit -m "커밋 메시지" 
```
- 커밋 메시지가 두 줄 일때 
```console
git commit -m "커밋 메시지 1" -m "커밋 메시지 2"  
```

## 깃 add와 commit을 한번에 하기 
```console
git commit -am "커밋 메시지"
```

## 깃 버전 히스토리 확인하기 
```console
git log 
```

## 프로젝트를 과거로 되돌리기 
### 리셋 (Reset)
- 이전 상태로 되돌아가거나 특정 커밋을 삭제할 때 사용 
- 과거로 돌아간 다음 이후 행적은 작업 내역에서 지워버리기 
- ⭐️ 리셋을 해버리면 협업 시 문제 발생, 다른 사람과 심각한 충돌이 일어남!!!! 
    - 공유된 커밋은 리버트를 사용해서 되돌리기⭐️

1. 해시값 구하기 
- commit 옆에 있는 문자열이 해시값, 해시값 복사하기 
```console
git log 
commit 415d8654a8405d940b7715e13bc6a9e7a62b426e 
```
2. 리셋하기 
```console
git reset --hard 해시값 
git reset --hard 415d8654a8405d940b7715e13bc6a9e7a62b426e 
```

### 리버트 (Revert)
- 이전 상태로 되돌아가면서 새로운 커밋을 생성하여 삭제된 내용을 되돌리는 데 사용 
- 되돌린 작업 내역을 되돌린 내역까지도 하나하나 기록으로 남길 필요가 있을 때 사용 
1. 해시값 구하기 
- commit 옆에 있는 문자열이 해시값, 해시값 복사하기 
```console
git log 
commit 415d8654a8405d940b7715e13bc6a9e7a62b426e 
```
2. 리버트하기 
```console
git revert 해시값 
git revert 415d8654a8405d940b7715e13bc6a9e7a62b426e
```

#### 리버트 충돌 시 
- git add나 git rm으로 상황을 해결 후 git revert --continue 명령 실행 
1. 문제가 되는 파일 삭제 
```console
rm 파일명 
```
2. 다시 리버트 
- continue 옵션은 충돌로 인해 중단되어 있는 revert 작업을 재개한다는 의미 
```console
git revert --continue
```

#### 커밋하지 않고 리버트하기 
- 리버트는 자동 커밋하지만 자동 커밋 안하기 
```console
git revert --no-commit 되돌릴 커밋 해시값 
```

## 브랜치 목록 보기 
```console 
git branch
```

## 브랜치 생성 
```console
git branch 브랜치명 
```

## 브랜치 이동
- 깃 2.23 버전부터 checkout 명령어가 switch와 restore로 분리
```console
git switch 브랜치명 
```

## 브랜치 생성하고 동시에 이동하기 
```console
git switch -c 브랜치명 
```

## 브랜치 이름 변경 
```console
git branch -m 기존브랜치이름 새브랜치이름 
```

## 브랜치 삭제 
```console
git branch -d 삭제할브랜치이름 
```

## 브랜치 강제 삭제 
```console
git branch -D 강제삭제할브랜치이름 
```

## 터미널 창에서 브랜치 작업 내역을 시각적으로 보기 
```console
git log --all --decorate --oneline --graph 
```

## 브랜치 합치기 

### 머지(Merge)
- 병합, 두가지를 이어 붙이기, 병합 과정에서 커밋하나가 생김 
- 브랜치의 사용 내역을 남겨 둘 필요가 있다면 머지 
1. 줄기 브랜치로 이동 
```console
git switch 브랜치명 
```
2. 머지 
```console
git merge 대상브랜치 
```
3. 필요하다면 불필요해진 대상브랜치 삭제 
```console
git branch -d 대상브랜치명 
```

### 리베이스(rebase)
- 브랜치를 다른 브랜치에 옮겨 붙이는 것 
- 작업 내역이 깔끔하게 한줄로 정리 
- 작업 내역을 깔끔하게 만드는 게 중요하다면 리베이스 
- ⭐️ 팀원들 간에 공유된 커밋에 대해서는 리베이스를 사용하지 않는게 좋음 ⭐️