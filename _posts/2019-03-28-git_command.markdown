---
layout: post
title:  "git 명령어 정리"
date:   2019-03-28 19:23:35 +0900
categories: 
- git
tags:
- git
- command
- 명령어
classes: wide
published: true
---



git을 사용하긴 하지만 아직도 많은 부분이 어색하네요. 아래는 가끔씩 찾는 부분을 정리하는 내용입니다.

## 브랜치 삭제

- local

```bash
git branch -d <branch_name>
git branch -D <branch_name> # 강제 삭제
```


- remote

```bash
git push <remote_name> --delete <branch_name>
```





## 브랜치 변경

- local

```
git checkout <branch_name>
```

- 작성 및 변경

```
git checkout -b <branch_name>
```





## cli 자동로그인(github)

```bash
git config credential.helper store
```


## add 취소

```bash
# 모두 추가
git add *
# 취소하기
git reset HEAD
# 특정 파일(CONTRIBUTING.md)
git reset HEAD CONTRIBUTING.md
```

## commit 취소

```bash
# log 확인
git log
# commit을 취소하고 unstaged 상태로 
git reset HEAD^
```


## 다른 branch 의 파일 가져오기

```bash
git branch master
git checkout <브랜치이름> <경로파일>
```


## git file 삭제

```bash
git rm <파일이름>
git rm --cached <파일이름>   # 원격저장소의 파일만 삭제
```


- [https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0#r_git_reset](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0#r_git_reset)
