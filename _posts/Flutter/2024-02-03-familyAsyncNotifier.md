---
layout: post
title: "[Flutter] 소셜 앱 '좋아요' 기능 구현: Riverpod FamilyAsyncNotifier를 활용한 상태관리"
date: 2024-02-04
categories: [Flutter]
tags: [flutter, riverpod]
---


*'좋아요' 기능 구현과정에서 왜 FamilyAsyncNotifier가 사용됐는지 회고해보려한다. 먼저, 비즈니스 로직, 데이터 액세스 로직은 간략하게는 다음과 같다.*

### VideoPost
<img src="/assets/img/Flutter/riverpod/SocialApp/videoPost_likeTapped.png" alt="" width=500>

*좋아요가 눌렀을 때, `_onLikeTaped`함수가 실행되며, `videoPostProvider(widget.videoData.id).notifier`로 상태관리 객체에 접근하여 `.likeVideo()`를 실행한다.*

<br>


### VideoPostViewModel
<img src="/assets/img/Flutter/riverpod/SocialApp/likeVideo_vm.png" alt="" width=500>

*Firestore의 유저 정보를 참조하여, `_videoRepository = ref.read(videoRepo)`를 통해 주입받은 레포지토리로 테이터 액세스 로직 `.likeVideo(_videoId, user!.uid)`을 수행한다.*

<br>

### VideoRepository
<img src="/assets/img/Flutter/riverpod/SocialApp/likeVideo_model.png" alt="" width=500>

*이를 통해, Firestore Database에 'likes' 콜렉션에 'userId'와 'videoId'를 갖는 도큐먼트가 생성된다.참고로, 'videoId'는 VideoModel의 데이터 구조 'id'로 추가하였고, 위 `.add()`메서드를 통해 Firestore에서 고유 'videoId'를 할당 받는다. 이를 통해, 사용자가 '좋아요'를 누를 때, 해당 사용자의 식별 정보(uid)를 서버로 전송하여, 어떤 사용자가 어떤 비디오에 '좋아요'를 했는지 기록할 수 있다.*

<br>
<br>
<br>




### *그럼 이제, VideoPostViewModel에 대해 파해쳐보자.*



```dart
import 'dart:async';

import 'package:TikTok/features/authentication/repos/authentication_repo.dart';
import 'package:TikTok/features/videos/models/%08video_post_state_model.dart';
import 'package:TikTok/features/videos/repos/videos_repo.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

class VideoPostViewModel extends FamilyAsyncNotifier<VideoPostStatus, String> {
  late final VideoRepository _videoRepository;
  late final _videoId;

  @override
  FutureOr<VideoPostStatus> build(String args) async {
    _videoId = args;
    _videoRepository = ref.read(videoRepo);
    final user = ref.read(authRepo).user;
    final isLiked = await ref.read(videoRepo).isLikedVideo(_videoId, user!.uid);
    final likesCount = await ref.read(videoRepo).fetchLikesCount(_videoId);
    print("isLiked: $isLiked");
    print("likesCount: $likesCount");

    state = AsyncValue.data(
        VideoPostStatus(isLiked: isLiked, likesCount: likesCount));
    return state.value!;
  }

  Future<void> toggleLikeVideo() async {
    final user = ref.read(authRepo).user;
    await _videoRepository.toggleLikeVideo(_videoId, user!.uid);
    state = AsyncValue.data(
      VideoPostStatus(
        isLiked: !state.value!.isLiked, // 현재 상태의 반대로 설정
        likesCount: state.value!.isLiked
            ? state.value!.likesCount - 1
            : state.value!.likesCount + 1, // 좋아요 상태에 따라 수 조정
      ),
    );
  }
}

final videoPostProvider =
    AsyncNotifierProvider.family<VideoPostViewModel, VideoPostStatus, String>(
  () => VideoPostViewModel(),
);

```



<br>
<br>


### *"왜 '좋아요' 기능 구현을 위해서 Riverpod 패키지의 'FamilyAsyncNotifier'를 사용해야할까?"*

<br>

'Notifier'는 하나의 상태만을 관리하기 때문에, 비디오 포스트 각각에 대한 독립적인 상태 관리에 한계가 있다. 반면, 'FamilyAsyncNotifier'는 파라미터를 기반으로 여러 상태 인스턴스를 관리할 수 있고, 이를 통해 동일한 타입의 객체나 로직을 여러 개의 독립된 인스턴스로 취급할 수 있다.

<br>

```dart
class VideoPostViewModel extends FamilyAsyncNotifier<void, String>
```
비디오 포스트 고유 ID에 따라 상태관리를 개별적으로 진행하기 위해 FamilyAsyncNotifier를 확장하고, 매개변수 타입을 `String`으로 정의한다.

```dart
  @override
  FutureOr<VideoPostStatus> build(String args) async {
    _videoId = args;
    _videoRepository = ref.read(videoRepo);
    final user = ref.read(authRepo).user;
    final isLiked = await ref.read(videoRepo).isLikedVideo(_videoId, user!.uid);
    final likesCount = await ref.read(videoRepo).fetchLikesCount(_videoId);
    print("isLiked: $isLiked");
    print("likesCount: $likesCount");

    state = AsyncValue.data(
        VideoPostStatus(isLiked: isLiked, likesCount: likesCount));
    return state.value!;
  }
```
> #### 의존성 주입 한번 더 집고 넘어가기
> ##### *의존성 주입(Dependency Injection, DI)은 컴포넌트(예: 클래스, 함수 등)가 직접 의존성(서비스, 데이터 모델 등)을 생성하는 대신 외부로부터 의존성을 받는 디자인 패턴이며, 이를 통해 컴포넌트의 결합도를 낮추고, 유연성 및 테스트 용이성을 향상시킨다.*

<br>

build함수를 통해 매개변수로 전달받은 비디오 고유 아이티를 `_videoId`에 할당하고, 데이터 접근을 위해 의존성을 주입한다.

```dart
  Future<void> toggleLikeVideo() async {
    final user = ref.read(authRepo).user;
    await _videoRepository.toggleLikeVideo(_videoId, user!.uid);
    state = AsyncValue.data(
      VideoPostStatus(
        isLiked: !state.value!.isLiked, // 현재 상태의 반대로 설정
        likesCount: state.value!.isLiked
            ? state.value!.likesCount - 1
            : state.value!.likesCount + 1, // 좋아요 상태에 따라 수 조정
      ),
    );
  }
```

`ref.read(authRepo).user`로 현재 사용자의 정보를 가져오며, VideoRepository 인스턴스의 `.toggleLikeVideo()`를 사용하여, '좋아요'기능을 적용하면, 다음과 같이 Firestore 데이터베이스에 'likes' 콜렉션과 해당 정보들이 생성된다.

<br>
<br>

### Firestore Database
<img src="/assets/img/Flutter/riverpod/SocialApp/firestore_likes.png" alt="" width=800>

<br>

### AsyncNotifierProvider.family?

```dart
final videoPostProvider =
    AsyncNotifierProvider.family<VideoPostViewModel, void, String>(
  () => VideoPostViewModel(),
);
```

### .family()와 제네릭
Riverpod에서 .family() 메서드는 특정 타입의 프로바이더를 생성할 때, 해당 프로바이더가 처리할 데이터 타입과, 프로바이더를 유니크하게 식별할 수 있는 매개변수의 타입을 지정해야 할 때 사용된다. ex) `NamedOfProvider.family<ReturnType, ParameterType>`

