---
id: git_1
title: git 명령어
sidebar_label: git 명령어
---

## ※ Push 과정

```
$ git init
$ git add . && git commit -m "커밋 내용"
$ git log
$ git remote add origin https://github.com/kyungyoonha/TIL
$ git push --set-upstream origin master    // 처음
```

## ※ Clone

```
$ git clone https://github.com/kyungyoonha/yoon.git .
$ npm install
```

## ※ PULL

```
$ git pull origin master
```

## ※ Remote

```
$ git remote -v
$ git remote add origin [레포지토리 url]
$ git remote rename [기존이름] [새로운이름]
$ git remote set-url origin https://github.com/user/repo2.git      // 원격 저장소 변경
```

## ※ add 취소

```
$ git status                             // add 된 파일들 확인
$ git restore --staged git/commend.md    // commend.md 파일 상태를 Unstage로 변경
```

## ※ commit 취소

```
$ git log                      // commit 목록 확인
$ git reset --soft "HEAD^"     // commit 취소 & 파일들 살려놓음 (staged)
$ git reset --mixed "HEAD^"    // commit 취소 & 파일들 살려놓음 (unstaged)
$ git reset "HEAD^"            // commit 취소 & 파일들 살려놓음 (unstaged)
$ git reset --hard "HEAD^"     // commit 취소 & 파일 삭제 (unstaged)

$ git reset --soft "HEAD~2"    // 최신 2개 commit 취소

// 참고 => "" 없이 사용하면 More? 이 나옴
// 로컬 또는 터미널에서 ^를 거르기 때문
```

## ※ commit 메세지

### 1-1 가장 최근 commit 메시지 수정

```
$ git commit -m "변경사항 메세지"     // 커밋 & 메시지 저장
$ git commit -amend                  // 커밋된 메세지 변경
$ git log                            // 커밋 내용 확인

// 참고: 수정    => i 끼워넣기
// 참고: 저장    => :w
// 참고: 나가기  => :q
```

### 1-2 기존 commit 메시지 수정

```
$ git rebase -i HEAD~3
// 바꾸고자 하는 commit의 맨 앞 pick을 edit으로 수정(i) 후 저장(:wq)
$ git commit -amend
// 내용 수정 후(i) 저장(:wq)
$ git rebase --continue
$ git push -f
```

## ※ push 취소하기

### 가장 최근 commit 취소하고 기존으로 되돌린 후 push(현재 작업도 지워짐)

-   참고. reflog 복잡하면 git log => id 값으로 reset 하면 된다
-   참고. 또는 \$ git reset [commit id] (목록에서 Id 값을 넣어줌)

```
$ git reset "HEAD^"                   // 가장 최근 commit 취소
$ git reflog                          // 커밋 목록 확인(브랜치와 HEAD가 가르켰던 커밋 기록)
$ git reset --hard HEAD@{1}           // 목록에서 번호를 고른 후 넣어줌 (0번이 제일 최신 -hard 옵션 주의)
$ git commit -m "다시 작업 후 커밋"    // 다시 작업후 커밋
$ git branch                          // 브랜치 이름 확인
$ git push origin [barnch name] -f    // 경고 무시하고 push
```

### 가장 최근 commit 취소하고 현재 작업중인 코드로 push

```
$ git reset "HEAD^"                   // 가장 최근 commit 취소
$ git reflog                          // 커밋 목록 확인(브랜치와 HEAD가 가르켰던 커밋 기록)
$ git reset HEAD@{1}                  // 번호 선택 후 넣어줌 (0번이 제일 최신 --mixed 옵션과 동일)
$ git commit -m "다시 작업 후 커밋"    // 다시 작업후 커밋
$ git branch                          // 브랜치 이름 확인
$ git push origin [barnch name] -f    // 경고 무시하고 push
```

## ※ env 히스토리 지우기

```
$ git rm env.local --cached
$ git rm env.staging --cached
$ git commit -m "Stopped tracking env.local, and env.staging"
```
