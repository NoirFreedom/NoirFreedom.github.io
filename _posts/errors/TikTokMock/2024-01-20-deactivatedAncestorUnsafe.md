---
layout: post
title: "[Flutter/Error] TikTokMock App -  Looking up a deactivated widget's ancestor is unsafe 오류"
date: 2024-01-20
categories: [Flutter]
tags: [flutter, personal projects, errors, problem solving]
---

##  "Looking up a deactivated widget's ancestor is unsafe" 오류

*"Firebase Auth를 구현하고, 앱 내, 회원가입에서 요구되는 정보를 입력 후, 등록 버튼을 누르면 다음과 같은 오류가 발생했다."*

<img src="/assets/img/Flutter_Projects/personal/tiktok/contextAsyncError2.png" alt="" width="600">

*"Flutter에서 위젯이 비활성화된 상태(deactivated state)에서 위젯 트리의 상위 위젯(ancestor)에 접근하려 할 때 오류가 발생했다는 것이다. 즉, 위젯이 위젯 트리에서 제거된 후, 해당 위젯의 BuildContext를 사용하여 상위 위젯에 접근하려하여 오류가 났다는 것."*

*"하지만 내 예상대로라면 정상적으로 회원가입을 진행했기 때문에, `AsyncValue.guard`의 `true`조건따라 아래 로직대로 **InterestsScreen**으로 이동해야 했었다."*

```dart
if (state.hasError) {
      showFirebaseErrorSnack(context, state.error);
    } else {
      context.goNamed(InterestsScreen.routeName);
    }
```




<br>

## 문제 해결

*"하지만 이 경우, 앱 사용자가 도중에 뒤로가기나 다른 화면으로 이동하게되면 `SignUp`메서드를 호출하는 **BirthdayScreen**이 제거될 수 있는데, 그렇게되면 `signUp` 메서드 내에서 `context`를 사용하는 시점에 이미 `context`가 속한 위젯이 위젯 트리에서 제거되어 비활성화된 상태가 될 수 있기 때문에 이런 오류가 출력된다는 것이다."*

<br>

### *"따라서 SignUpViewModel의 `signUp`메소드에서 `context.goNamed(InterestsScreen.routeName);`를 처리하는 대신,`else`구문을 삭제하고, BirthdayScreen에서 `_onNextTap`을 비동기 처리하고 `await`을 걸어둠으로써 `signUp`메소드가 수행된 후 BirthdayScreen에서 라우팅이 적용될 수 있도록 다음과 같이 수정하여 문제를 해결하였다."*

<br>



### 기존의 SignUpViewModel
<img src="/assets/img/Flutter_Projects/personal/tiktok/contextAsyncError5.png" alt="" width="600">

### 수정된 SignUpViewModel
<img src="/assets/img/Flutter_Projects/personal/tiktok/contextAsyncError4.png" alt="" width="600">

---

### 기존의 BirthdayScreen `_onNextTap`메소드
<img src="/assets/img/Flutter_Projects/personal/tiktok/contextAsyncError3.png" alt="" width="600">

### 수정된 BirthdayScreen `_onNextTap`메소드
<img src="/assets/img/Flutter_Projects/personal/tiktok/contextAsyncError5.png" alt="" width="600">

