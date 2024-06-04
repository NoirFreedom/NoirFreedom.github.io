---
layout: post
title: "[개발환경] GitLab SSH키 등록"
date: 2024-06-04
categories: [Dev_Environment_Setup]
tags: [dev environment setup]
---

## 기존 SSH 키를 새로 생성하고 GitLab에 등록하는 방법

### 1. 기존 SSH 키 삭제 (옵션)

```bash
rm ~/.ssh/id_rsa ~/.ssh/id_rsa.pub
```

### 2. 새로운 SSH 키 생성

1. 터미널을 엽니다.
2. 다음 명령어로 SSH 키를 생성합니다.

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

* `your_email@example.com`은 GitLab 계정에 등록된 이메일 주소로 변경해야 합니다.
* 파일 경로와 이름 설정 시 기본값을 사용하려면 Enter 키를 누릅니다.
* 암호를 설정하려면 원하는 암호를 입력하고, 암호 없이 사용하려면 Enter 키를 누릅니다.

### 3. SSH 공개 키를 GitLab에 추가

1. 공개 키를 클립보드에 복사합니다.

```bash
pbcopy < ~/.ssh/id_rsa.pub
```

2. GitLab에 로그인하고, 프로필 사진을 클릭한 후 "Preferences"를 선택합니다.
3. 왼쪽 사이드바에서 "SSH Keys"를 클릭합니다.
4. "Key" 필드에 클립보드에 복사한 공개 키를 붙여넣고 "Title" 필드에 키의 이름을 입력한 후 "Add key" 버튼을 클릭합니다.

### 4. SSH 에이전트 구성

1. SSH 에이전트가 실행 중인지 확인하고, 실행 중이 아니면 시작합니다.

```bash
eval "$(ssh-agent -s)"
```

2. SSH 에이전트에 새 키를 추가합니다.

```bash
ssh-add ~/.ssh/id_rsa
```

### 5. SSH 설정 파일 구성 (옵션)

1. 설정 파일을 엽니다.

```bash
nano ~/.ssh/config
```

2. GitLab에 대한 설정을 추가합니다.

```
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_rsa
```

3. 설정 파일을 저장하고 종료합니다 (Ctrl + X, Y, Enter).

### 6. Git 명령 다시 시도

```bash
git clone git@gitlab.com:exflyer/mayo-web-dms.git
```

**문제 해결:**

만약 여전히 문제가 발생한다면, 다음 사항들을 확인해보세요:

* SSH 키가 올바르게 추가되었는지.
* SSH 에이전트가 실행 중인지.
* GitLab 설정에서 SSH 키가 올바르게 등록되었는지.