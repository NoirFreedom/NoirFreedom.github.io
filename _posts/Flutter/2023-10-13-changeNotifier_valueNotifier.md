---
layout: post
title: "[Flutter] State Management - ChangeNotifier & ValueNotifier"
date: 2023-10-13
categories: [Flutter]
tags: [flutter]
---

저번 포스트에서 다룬 inheritedWidget에선 상태가 변경될 때 마다 필연적으로 화면이 재빌드 된다는 문제가 있었다. 이번 포스트에선 이것을 ChangeNotifier과 ValueNotifier로 보완해보려 한다.

## ChangeNotifier

`ChangeNotifier`를 사용하면 상태가 변경될 때마다 관련된 리스너에게 알림을 보내는 방식으로 특정 위젯만 재빌드 할 수 있다. 이는 `setState()`를 호출하여 전체 `StatefulWidget`을 재빌드하는 리소스를 절약하고 앱의 성능을 향상시킬 수 있기에 더 효율적이다. 

아래 예시에서는 `VideoConfig`라는 클래스가 `ChangeNotifier`를 확장하여 `autoMute` 상태를 관리하고, `toggleAutoMute()` 메서드는 `autoMute`의 값을 토글하며 `notifyListeners()`를 호출하여 해당 변화를 리스너들에게 알린다. 이 경우 `AnimatedBuilder`를 사용하여 `SwitchListTile` 위젯을 `autoMute` 값에 따라 재빌드한다. 


### ChangeNotifier을 사용한 예시코드:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

// 상태 관리 클래스
class VideoConfig extends ChangeNotifier {
  bool autoMute = false; // 비디오 자동 음소거 상태

  // 자동 음소거 상태를 토글하고 리스너들에게 알림
  void toggleAutoMute() {
    autoMute = !autoMute;
    notifyListeners();
  }
}

// 앱의 루트 위젯
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'ChangeNotifier Example',
      home: HomeScreen(),
    );
  }
}

// 홈스크린 위젯
class HomeScreen extends StatelessWidget {
  final VideoConfig videoConfig = VideoConfig();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ChangeNotifier Example')),
      body: AnimatedBuilder(
        animation: videoConfig, // VideoConfig 객체를 사용
        builder: (context, child) {
          return SwitchListTile.adaptive(
            value: videoConfig.autoMute, // VideoConfig의 autoMute 값을 사용
            onChanged: (value) {
              videoConfig.toggleAutoMute(); // SwitchListTile 변경 시 VideoConfig를 토글
            },
            title: const Text("Mute video"),
            subtitle: const Text("Videos will be muted by default."),
          );
        },
      ),
    );
  }
}
```

위 코드에서 `AnimatedBuilder`는 `videoConfig` 객체의 상태에 따라 `SwitchListTile` 위젯을 재빌드한다. 사용자가 스위치를 토글할 때 `toggleAutoMute` 메서드가 호출되고, `ChangeNotifier`에 의해 `AnimatedBuilder`가 자동으로 위젯을 업데이트하는 것이다(.addListener도 사용 가능하다). 전체 화면이 아닌 `SwitchListTile` 위젯만 재빌드되고, 리스너에 등록된 위젯만을 대상으로 최소한의 재빌드를 수행하여 성능을 향상시켜 상태 관리를 더 세분화하여 구조화할 수 있도록 돕는다.

<br>

## ValueNotifier

그럼 `ValueNotifier`는 ChangeNotifier과 어떤 차이가 있을까? `ValueNotifier`는 단일 값을 가진 `ChangeNotifier`의 특화된 버전으로, 이를 사용하면 상태 관리를 보다 간결하게 할 수 있다. `ValueNotifier`는 특정 값이 변경될 때 리스너들에게 알리고, `ValueListenableBuilder` 위젯을 사용하여 변경된 값을 기반으로 위젯을 재빌드한다. 이 방식은 상태가 단일 값으로 구성되어 있을 때 매우 유용하다.

### ValueNotifier을 사용한 예시코드:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

// 앱의 루트 위젯
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'ValueNotifier Example',
      home: HomeScreen(),
    );
  }
}

// 홈스크린 위젯
class HomeScreen extends StatelessWidget {
  // ValueNotifier 객체를 생성하고 기본값으로 false를 할당
  final ValueNotifier<bool> autoMuteNotifier = ValueNotifier<bool>(false);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ValueNotifier Example')),
      body: ValueListenableBuilder<bool>(
        valueListenable: autoMuteNotifier, // ValueNotifier 객체를 리스너로 설정
        builder: (context, autoMute, child) {
          return SwitchListTile.adaptive(
            value: autoMute, // ValueNotifier의 현재 값으로 스위치의 상태를 설정
            onChanged: (value) {
              // 스위치의 상태가 변경될 때 ValueNotifier의 값을 업데이트
              autoMuteNotifier.value = value;
            },
            title: const Text("Mute video"),
            subtitle: const Text("Videos will be muted by default."),
          );
        },
      ),
    );
  }
}
```

위 코드에서 `autoMuteNotifier`는 `ValueNotifier<bool>`로 선언되어 있으며, 스위치의 상태를 관리한다. 사용자가 스위치를 토글할 때마다 `ValueNotifier`의 `value`가 변경되고, 이에 대한 알림이 발생하고, `ValueListenableBuilder`는 이 알림을 감지하여 스위치의 상태에 해당하는 UI만을 재빌드한다. 이렇게 함으로써, 변경된 상태에 관련된 위젯만 최신 상태로 업데이트되고 나머지 UI는 그대로 유지된다.

`ValueNotifier`를 사용하면 상태 관리를 보다 직관적으로 할 수 있으며, 이는 특히 단일 상태 값에 대한 변경 사항을 관리할 때 매우 효과적이다.

<br>

## ChangeNotifier와 ValueNotifier의 단점

`ChangeNotifier`와 `ValueNotifier`는 Flutter에서 상태 관리를 위해 널리 사용되지만, 몇 가지 단점이 있다:

### ChangeNotifier의 단점:

1. **리소스 소비**: `ChangeNotifier`는 리스너들에게 알림을 보낼 때 모든 리스너를 순회합니다. 이는 많은 수의 리스너가 있을 경우, 알림을 보내는데 많은 리소스를 소비할 수 있다.

2. **불필요한 재빌드**: `notifyListeners()` 메서드가 호출되면, 리스너로 등록된 모든 위젯을 재빌드한다. 이는 때때로 불필요한 재빌드를 유발할 수 있으며, 성능 저하를 일으킬 수 있다.

3. **상태 관리 분산**: `ChangeNotifier`를 사용할 때 상태가 여러 클래스에 분산될 수 있으며, 이는 상태의 흐름을 추적하기 어렵게 만들 수 있다.

4. **메모리 누수**: 리스너를 제대로 관리하지 않으면 메모리 누수가 발생할 수 있다. 예를 들어, 위젯이 제거될 때 리스너를 해제하지 않으면, 가비지 컬렉터가 해당 위젯을 메모리에서 해제하지 못할 수 있다.

5. **Type Safety 부족**: `ChangeNotifier`는 어떤 타입의 변경도 알릴 수 있지만, 어떤 데이터가 변경되었는지에 대한 정보는 제공하지 않기때문에 이로 인해 타입 안전성이 감소할 수 있다.

<br>

### ValueNotifier의 단점:

1. **단일 값에 제한**: `ValueNotifier`는 단일 값의 변경만을 추적하며, 복잡한 상태를 가진 애플리케이션에는 적합하지 않을 수 있다.

2. **상태 변경 복잡성**: 복잡한 상태를 `ValueNotifier`로 관리하려고 할 때, 여러 `ValueNotifier`를 동시에 관리하고 동기화하는 것이 복잡하고 어려울 수 있다.

3. **효율성 문제**: `ValueNotifier`의 값이 자주 변경되는 경우, 불필요하게 많은 재빌드가 발생할 수 있으며, 이는 성능 저하를 일으킬 수 있다.

4. **상태 관리 패턴**: `ValueNotifier`는 상태 관리 패턴이 명확하지 않기 때문에, 큰 규모의 애플리케이션에서는 상태 관리를 위한 보다 강력한 솔루션(예: Provider, Riverpod, Bloc)이 필요할 수 있다.

<br>

> *"그럼으로, 애플리케이션의 복잡성이 증가함에 따라 더 발전된 상태 관리 솔루션을 고려해야한다."*