---
layout: post
title: "[Flutter] didChangeDependencies 메서드"
date: 2023-09-14
categories: [Flutter]
tags: [flutter]
---




`didChangeDependencies` 메서드는 Flutter의 `StatefulWidget` 라이프사이클 내에서 중요한 역할을 수행한다. 이 메서드는 위젯의 의존성이 변경될 때마다 호출되며, 특히 `InheritedWidget`의 변경을 감지할 때 유용하게 사용된다.

### `didChangeDependencies` 사용 시점과 목적:

1. **위젯의 의존성이 변경될 때 반응**: Flutter에서 위젯은 종종 `InheritedWidget`을 통해 데이터를 상속받는데, `didChangeDependencies`는 이 상속받은 데이터가 변경될 때마다 호출된다. 예를 들어, `MediaQuery`, `Locale`, `Theme` 등이 변경되었을 때 이 메서드가 호출된다.

2. **초기화 로직에 `context`가 필요할 때**: `initState` 메서드는 위젯이 위젯 트리에 완전히 추가되기 전에 호출되므로, 이 시점에서는 `context`를 통해 데이터에 접근하는 것이 안전하지 않다. 만약 초기화 로직이 `context`에 의존하는 데이터에 접근해야 한다면, `didChangeDependencies`에서 이러한 로직을 처리하는 것이 적합하다.

3. **여러 의존성에 반응해야 할 때**: 하나의 위젯이 여러 `InheritedWidget`에 의존하고 있을 때, 이러한 의존성 중 하나라도 변경되면 `didChangeDependencies`가 호출된다. 이를 통해 위젯은 다양한 의존성의 변경에 적절히 반응할 수 있다.

### 사용 예시:

```dart
class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  ThemeData themeData;

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    // Theme.of(context)를 사용하여 현재 테마 데이터에 접근
    themeData = Theme.of(context);
  }

  @override
  Widget build(BuildContext context) {
    // build 메서드에서는 themeData를 사용할 수 있음
    return Container(color: themeData.primaryColor);
  }
}
```

이 예시에서 `MyWidget`은 현재 `Theme` 데이터에 의존하고 있다. 테마가 변경될 때마다 `didChangeDependencies`가 호출되어 `themeData`가 업데이트되고, `build` 메서드는 항상 최신의 테마 정보를 반영하여 UI를 빌드한다.

### 주의 사항:

- `didChangeDependencies`는 의존성이 변경될 때마다 여러 번 호출될 수 있으므로, 성능을 고려하여 필요한 경우에만 상태를 업데이트하거나 데이터를 재계산해야 한다.


<br>
<br>
<br>
<br>


#### *라이프사이클

Flutter의 라이프사이클은 앱이나 위젯이 생성되고 소멸되기까지의 일련의 단계나 상태를 의미한다. 특히, 위젯 라이프사이클은 위젯이 생성(create), 마운트(mount), 업데이트(update), 언마운트(unmount), 소멸(destroy)되는 과정을 포함한다. 이 과정은 위젯의 생성자 호출부터 `dispose` 메서드 호출까지의 과정을 포함하며, 이러한 라이프사이클을 관리하는 것은 Flutter 앱의 성능과 안정성에 중요한 영향을 미친다.

#### *의존성

의존성(dependency)은 플러터에서 특정 위젯이 다른 요소나 객체에 의존하는 것을 의미한다. 예를 들어, 위젯이 `Theme` 데이터에 의존하는 경우, `Theme`의 변경에 반응하여 위젯이 업데이트될 수 있다. 위젯은 또한 다른 위젯의 상태, 앱 설정, 또는 시스템 환경(예: 화면 크기, 방향)과 같은 다양한 요소에 의존할 수 있다.

#### *InheritedWidget

`InheritedWidget`은 Flutter에서 위젯 트리를 통해 데이터를 효율적으로 전달하는 방법을 제공하는 특수한 종류의 위젯이다. 이 위젯을 사용하면 위젯 트리의 상위에서 데이터를 정의하고, 하위 위젯들이 그 데이터에 쉽게 접근할 수 있게 된다.

하위 위젯은 `context`를 통해 `InheritedWidget`에 접근할 수 있으며, `InheritedWidget`의 데이터가 변경될 때마다 하위 위젯은 이 변경을 감지하고 적절히 반응할 수 있다. `InheritedWidget`은 주로 공통의 데이터(예: 테마, 로케일 정보)를 위젯 트리 전체에 걸쳐 공유할 때 사용된다.

##### InheritedWidget 예시:

```dart
class MyInheritedWidget extends InheritedWidget {
  final String data;

  const MyInheritedWidget({
    Key? key,
    required this.data,
    required Widget child,
  }) : super(key: key, child: child);

  static MyInheritedWidget? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
  }

  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    return data != oldWidget.data;
  }
}
```

이 예시에서 `MyInheritedWidget`은 `data`라는 정보를 가지고 있으며, 이 위젯을 사용하는 하위 위젯은 `MyInheritedWidget.of(context)`를 호출하여 `data`에 접근할 수 있다. `data`가 변경되면 `updateShouldNotify`가 `true`를 반환하고, 이에 의존하는 하위 위젯들은 자동으로 재빌드된다.