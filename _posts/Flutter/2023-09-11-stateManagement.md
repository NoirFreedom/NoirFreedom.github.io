---
layout: post
title: "[Flutter] 상태 관리(State Management)"
date: 2023-09-11
categories: [Flutter]
tags: [flutter]
---


Flutter를 사용하면서 상태 관리와 의존성 주입에 대해 고민이 많았는데, 최근에 GetX와 Riverpod를 깊게 탐구해보았다. 이 두 라이브러리는 Flutter에서 매우 인기 있는 선택지이며, 각기 다른 장점과 특성을 가지고 있다.

**GetX는 정말 사용하기 간편하면서도 강력한 기능을 제공한다.** 먼저, 상태 관리 측면에서 `.obs`를 사용하여 반응형 상태를 쉽게 생성할 수 있다는 점이 인상적이다. 예를 들어, 단순한 카운터 앱에서는 `final count = 0.obs;`와 같이 선언하고, `Obx()` 위젯을 사용해 UI에 상태 변화를 자동으로 반영할 수 있다. 

GetX의 의존성 관리 또한 훌륭하다. `Get.put()`, `Get.lazyPut()`, `Get.find()`와 같은 메서드를 통해 의존성을 쉽게 주입하고 관리할 수 있다. 라우팅 관리도 마찬가지로, `Get.to()`, `Get.off()`, `Get.back()` 등을 사용하여 간편하게 화면 이동을 처리할 수 있다. 이런 기능들 덕분에 GetX는 복잡하지 않으면서도 매우 효율적인 코드 작성을 가능하게 해준다.


### GetX 예시:
```dart
class CounterController extends GetxController {
  var count = 0.obs;

  void increment() => count++;
}

// ...

final CounterController controller = Get.put(CounterController());

// ...

Obx(() => Text('Count value is ${controller.count.value}'));
RaisedButton(
  onPressed: () {
    controller.increment();
  },
  child: Text('Increment'),
)

```

반면에, **Riverpod는 유연성과 테스트 용이성에서 두각을 나타낸다.** Riverpod의 가장 큰 특징 중 하나는 context에 의존하지 않는다는 것이다. 이는 어디서나 데이터에 접근할 수 있게 해주며, 특히 큰 프로젝트에서 그 강점이 빛난다. 또한, `StateProvider`, `FutureProvider`, `StreamProvider` 등 다양한 형태의 Provider를 생성할 수 있어 상태 관리에 매우 유연하다.

Riverpod의 테스트 용이성 또한 주목할 만하다. 상태 및 의존성을 쉽게 모의할 수 있어서 테스트가 훨씬 수월해진다. 이는 크고 복잡한 앱에서 매우 중요한 요소다.

개인적으로, GetX 예시로는 `Obx(() => Text('Count value is ${count.value}'));`와 같은 코드를 사용해본 경험이 있고, Riverpod에서는 `final counterProvider = StateProvider((ref) => 0);`와 같은 Provider를 정의하고 `Consumer` 위젯을 사용하여 상태를 관찰하는 방식이 흥미로웠다.

이 두 라이브러리를 사용하면서 느낀 점은, 각각의 사용 사례와 프로젝트의 요구 사항에 따라 최적의 선택이 달라진다는 것이다. GetX는 빠르고 쉬운 개발을 원할 때 효과적이며, Riverpod는 더 유연하고 테스트하기 쉬운 환경을 원할 때 적합하다. 결국, 상황에 맞게 적절한 도구를 선택하는 것이 중요하다는 것을 다시 한번 깨달았다. Flutter 개발자로서 이런 다양한 옵션을 탐색하고 학습하는 것은 정말 흥미로운 경험이다.

### Riverpod 예시:
```dart
final counterProvider = StateProvider((ref) => 0);

// ...

Consumer(builder: (context, watch, _) {
  final count = watch(counterProvider).state;
  return Column(
    children: [
      Text('Count value is $count'),
      RaisedButton(
        onPressed: () => context.read(counterProvider).state++,
        child: Text('Increment'),
      ),
    ],
  );
});

```

