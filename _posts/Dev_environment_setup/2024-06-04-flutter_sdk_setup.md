---
layout: post
title: "[개발환경] Flutter SDK Setup"
date: 2024-06-04
categories: [Dev Environment Setup]
tags: [dev environment setup]
---

## Homebrew를 사용하여 macOS에 Flutter SDK 설치 및 VS Code 설정하기

Homebrew는 macOS 패키지 관리자로, Flutter SDK 설치 및 관리를 간편하게 해줍니다.

### 1. Homebrew 설치

터미널에서 다음 명령어를 실행하여 Homebrew를 설치합니다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. Flutter SDK 설치

1. **Flutter 설치:**

```bash
brew install --cask flutter
```

2. **환경 변수 설정:** 터미널에서 다음 명령어를 실행하여 Flutter SDK 경로를 확인합니다.

```bash
brew info flutter
```

출력된 경로를 복사하여 아래 명령어에 붙여넣고 실행합니다.

```bash
nano ~/.zshrc  # zsh 사용 시
nano ~/.bash_profile  # bash 사용 시
```

파일이 열리면 다음 줄을 추가합니다.

```bash
export PATH="$PATH:[붙여넣은 Flutter SDK 경로]/flutter/bin"
```

파일을 저장하고 닫습니다 (Ctrl + X, Y, Enter).

3. **경로 적용:**

```bash
source ~/.zshrc  # zsh 사용 시
source ~/.bash_profile  # bash 사용 시
```

4. **Flutter 설치 확인:**

```bash
flutter doctor
```

### 3. VS Code에 Flutter 및 Dart 확장 설치

1. Visual Studio Code를 엽니다.
2. 확장 탭에서 "Flutter"를 검색하고 설치합니다. Dart 확장도 함께 설치됩니다.

### Android Studio 설정 (선택 사항)

Android 개발을 위해 Android Studio를 설치하고 설정해야 합니다.

1. **Android Studio 실행:**

```bash
open -a "Android Studio"
```

2. **설정 마법사:** 안내에 따라 설정을 완료합니다.
3. **Android SDK 경로 설정:** Android Studio 설정에서 SDK 경로를 확인하고, 터미널에서 다음 명령어를 실행합니다.

```bash
flutter config --android-sdk [Android SDK 경로]
```

### Xcode 설정 (선택 사항)

iOS 개발을 위해 Xcode를 설치하고 설정해야 합니다.

1. App Store에서 Xcode를 설치합니다.
2. Xcode를 실행하고 설정을 완료합니다.

### Flutter Doctor 재확인

모든 설정이 완료되면 `flutter doctor` 명령어를 다시 실행하여 문제가 없는지 확인합니다.

```bash
flutter doctor
```

**문제 해결:**

* **Android 관련 오류:** `flutter doctor --android-licenses` 명령어를 실행하고 "y"를 입력하여 라이선스에 동의합니다.
* **Xcode 관련 오류:** Xcode를 실행하고 "Preferences" > "Platforms"에서 필요한 시뮬레이터를 설치합니다.