---
layout: post
title: "[Flutter] State Management - InheritedWidget with StatefulWidget"
date: 2023-10-08
categories: [Flutter]
tags: [flutter]
---

## InheritedWidget / StatefullWidget

### InheritedWidget 이란?

`InheritedWidget`은 Flutter에서 상태 관리를 위해 사용되는 특별한 종류의 위젯이다. 이 위젯은 데이터를 공유하고, 위젯 트리를 통해 하위 위젯에 데이터를 전달하는 데 사용된다. `InheritedWidget`을 사용하면 위젯 트리의 어느 위치에서든지 필요한 데이터에 접근할 수 있게 된다.

#### 기본 원리:

1. **데이터 공유**: `InheritedWidget`은 위젯 트리 상의 모든 자식 위젯에게 데이터를 제공할 수 있다. 이를 통해 중앙 집중식으로 데이터를 관리하고 공유할 수 있다.

2. **효율적인 데이터 접근**: 일반적으로 데이터를 전달하기 위해 여러 위젯을 거쳐야 하지만, `InheritedWidget`을 사용하면 직접적으로 필요한 데이터에 접근할 수 있어 코드가 더 간결해진다.

3. **위젯 트리 최적화**: `InheritedWidget`은 종속된 자식 위젯에게만 재빌드 신호를 보낼 수 있어, 불필요한 재빌드를 줄일 수 있습니다. 이를 통해 앱의 성능을 향상시킬 수 있습니다.

<br>

### 예시 코드:

아래 예제에서 `MyInheritedWidget`이라는 `InheritedWidget`을 사용하여 데이터를 관리하고, 하위 위젯에서 이 데이터에 접근한다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // MyInheritedWidget을 사용하여 데이터를 전달하고 앱 전체에 걸쳐 공유
    return MaterialApp(
      title: 'InheritedWidget Demo',
      home: MyInheritedWidget(
        myData: MyData("Hello from InheritedWidget!"), // 공유할 데이터
        child: Scaffold(
          appBar: AppBar(title: Text('InheritedWidget Example')),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ChildWidgetA(), // 자식 위젯 A
                ChildWidgetB(), // 자식 위젯 B
              ],
            ),
          ),
        ),
      ),
    );
  }
}

class MyData {
  final String message; // 공유할 데이터

  MyData(this.message);
}

class MyInheritedWidget extends InheritedWidget {
  final MyData myData; // 상속받을 데이터

  // 생성자에서 필요한 데이터와 자식 위젯을 받음
  MyInheritedWidget({Key? key, required this.myData, required Widget child})
      : super(key: key, child: child);

  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    // 데이터가 변경되었는지 확인
    return myData.message != oldWidget.myData.message;
  }

  // 데이터에 접근하기 위한 정적 메서드
  static MyData of(BuildContext context) {
    final MyInheritedWidget? result = context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
    assert(result != null, 'No MyInheritedWidget found in context');
    return result!.myData;
  }
}

class ChildWidgetA extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // MyInheritedWidget에서 데이터를 가져옵니다.
    final myData = MyInheritedWidget.of(context).message;

    return Text("Message from InheritedWidget: $myData");
  }
}

class ChildWidgetB extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 동일한 방식으로 데이터에 접근
    final myData = MyInheritedWidget.of(context).message;

    return Text("Another message: $myData");
  }
}
```

이 코드에서 `MyInheritedWidget`은 `myData` 객체를 관리하고, `ChildWidgetA`와 `ChildWidgetB`는 `MyInheritedWidget.of(context)`를 통해 이 데이터에 접근합니다. 이 예시에서 `updateShouldNotify` 메서드는 데이터가 변경될 때만 자식 위젯에게 재빌드 신호를 보내도록 구성되어 있습니다.




<br>

### `InheritedWidget`을 확장하여 사용 시, 유의사항

1. **`updateShouldNotify` 메서드**: `InheritedWidget`을 확장하는 클래스는 `updateShouldNotify` 메서드를 오버라이드해야 한다. 이 메서드는 데이터가 변경되었을 때 하위 위젯들이 재빌드되어야 하는지 여부를 결정한다. `true`를 반환하면, 이 위젯에 의존하는 하위 위젯들이 재빌드된다.

2. **생성자와 `child`**: `InheritedWidget`을 확장할 때는 `child` 위젯을 받는 생성자를 제공해야 한다. 이 `child`는 `InheritedWidget` 아래에 위치하는 위젯 트리를 나타낸다.

3. **`dependOnInheritedWidgetOfExactType` 사용**: 하위 위젯에서 상속받은 위젯의 데이터에 접근하기 위해 `context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>()` 메서드를 사용한다. 이 메서드를 호출하면 해당 타입의 가장 가까운 상위 `InheritedWidget`의 인스턴스를 반환한다.

<br>
<br>

## InheritedWidget + StatefulWidget

### 예시 코드

아래 예시에선 `MyInheritedWidget`이라는 `InheritedWidget`을 생성하고, 상태 변경을 위해 `StatefulWidget`을 사용한다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MyStatefulWidget(),
    );
  }
}

// StatefulWidget을 사용하여 상태 관리
class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  String message = "Initial Message";

  void updateMessage(String newMessage) {
    setState(() {
      message = newMessage;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MyInheritedWidget(
      message: message,
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          MessageWidget(), // MyInheritedWidget에서 데이터를 사용하는 위젯
          ElevatedButton(
            onPressed: () => updateMessage("Updated Message"),
            child: Text("Update Message"),
          ),
        ],
      ),
    );
  }
}

// InheritedWidget 확장
class MyInheritedWidget extends InheritedWidget {
  final String message;

  const MyInheritedWidget({
    Key? key,
    required this.message,
    required Widget child,
  }) : super(key: key, child: child);

  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    return message != oldWidget.message;
  }

  static MyInheritedWidget? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
  }
}

// MyInheritedWidget에서 데이터를 사용하는 위젯
class MessageWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final inheritedWidget = MyInheritedWidget.of(context);

    return Text(
      inheritedWidget?.message ?? "No message",
      style: TextStyle(fontSize: 24),
    );
  }
}
```

이 코드에서 `MyStatefulWidget`은 메시지를 관리하고 `MyInheritedWidget`에 전달한다. `MessageWidget`은 `MyInheritedWidget`에서 메시지를 받아 표시하고, "Update Message" 버튼을 누르면 메시지가 업데이트되며, `MyInheritedWidget`의 `updateShouldNotify`는 새로운 메시지를 감지하여 `MessageWidget`을 재빌드한다.

<br>

### 문제점


1. **복잡성**: `InheritedWidget`은 특히 새로운 Flutter 개발자에게는 이해하기 어려울 수 있습니다. 데이터를 전달하는 방식이 직관적이지 않을 수 있으며, `InheritedWidget`을 적절하게 사용하기 위해서는 Flutter의 위젯 트리와 상태 관리에 대한 깊은 이해가 필요합니다.

2. **보일러플레이트 코드**: `InheritedWidget`을 사용하려면 추가적인 설정 코드가 필요합니다. 예를 들어, `updateShouldNotify` 메서드를 구현해야 하며, 데이터가 변경될 때마다 하위 위젯들에게 알리는 로직을 작성해야 합니다.

3. **제한된 사용 사례**: `InheritedWidget`은 주로 위젯 트리의 상위에서 하위로 데이터를 전달하는 데 사용됩니다. 이는 양방향 데이터 흐름이나, 보다 복잡한 상태 관리 시나리오에는 적합하지 않을 수 있습니다.

4. **재빌드 효율성 문제**: `InheritedWidget`은 종속된 위젯이 데이터의 변경을 감지할 때마다 재빌드될 수 있습니다. 이는 때때로 불필요한 재빌드를 유발할 수 있으며, 특히 많은 데이터를 관리하거나, 복잡한 위젯 트리에서는 성능 문제를 일으킬 수 있습니다.

5. **상태 변경 관리의 복잡성**: `InheritedWidget`은 불변의 데이터를 전달하는 것에는 적합하지만, 가변적인 상태를 관리하기 위해서는 추가적인 상태 관리 솔루션(예: `Provider`, `Bloc`, `Redux` 등)이 필요할 수 있습니다.

`Provider`는 `InheritedWidget`을 기반으로 하지만, 사용이 더 쉽고, 추가적인 기능을 제공하기때문에 많은 Flutter 개발자들이 `InheritedWidget` 대신 `Provider`와 같은 보다 강력하고 유연한 상태 관리 패키지를 선호하는 경우가 많다고 한다.
