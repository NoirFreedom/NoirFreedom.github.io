---
layout: post
title: "[Flutter] Basics 개념정리"
date: 2023-08-21
categories: [Flutter]
tags: [flutter]
---







# **Flutter란?**

*"Flutter는 구글에서 개발한 오픈 소스 UI 프레임워크로, 모바일 애플리케이션과 웹 애플리케이션을 위한 사용자 인터페이스를 빌드하고 개발하는 데 사용되는 플랫폼이다"*

<br>

### **Flutter의 주요 특징**

- **위젯 기반 아키텍처:** Flutter는 UI를 구성하는 모든 요소를 위젯(widget)이라는 작은 단위로 나누어 관리한다.

- **빠른 개발 및 핫 리로딩:** Flutter는 핫 리로딩(Hot Reload) 기능을 통해 애플리케이션의 변경 사항을 빠르게 반영하고 확인이 가능하다.

- **고성능:** Flutter는 네이티브 수준의 성능을 제공하며, Skia 그래픽 엔진을 사용하여 모든 플랫폼에서 뛰어난 성능과 부드러운 애니메이션을 구현할 수 있다.

- **단일 코드베이스:** Flutter는 동일한 코드베이스를 사용하여 웹, iOS, Android 애플리케이션을 개발한다.

- **다양한 위젯 및 스타일:** Flutter는 다양한 내장 위젯과 커스텀 가능한 스타일을 제공하여 다양한 디자인 요소를 구현이 가능하다.

- **강력한 커뮤니티:** Flutter는 활발한 개발자 커뮤니티와 다양한 리소스를 제공한다.

<br>

### **Widget이란?**

*"Flutter에서 **Widget** 은 사용자 인터페이스(UI)를 구성하는 기본적인 빌딩 블록으로, 화면에 그려지는 모든 것을 나타내며, 버튼, 텍스트, 이미지, 레이아웃 등 다양한 UI 요소를 나타내기 위해 사용되고, 또 계층 구조로 조합될 수 있다. 즉, 하나의 위젯이 다른 위젯의 부모가 되며, 이러한 계층 구조를 통해 복잡한 UI를 구성할 수 있으며 이를 통해 위젯 간의 중첩과 조합을 통해 원하는 디자인과 동작을 구현할 수 있다."*

<br>


- **StatelessWidget:** 상태가 없는 위젯으로, 생성된 후 변하지 않는 UI 요소를 나타낸다(텍스트, 아이콘 등)

- **StatefulWidget:** 상태를 가지는 위젯으로, 사용자의 입력이나 앱의 상태에 따라 변화하는 UI 요소를 나타낸다(사용자의 터치에 반응하는 버튼이나 애니메이션 등)


<br>


## **StateManagement**

<br>


#### **Q. Flutter에서 State는 왜 필요한가?**

*"Flutter에서 상태(State)는 앱의 동적인 부분을 나타내며, 사용자와의 상호작용에 따라 UI를 업데이트하고 제어하는 데 중요한 역할을 하기때문"*

#### **Flutter에서 상태가 필요한 이유**

- **동적인 UI:** 앱의 UI는 사용자의 입력, 외부 데이터, 앱 내부에서 발생하는 이벤트 등에 의해 동적으로 변할 수 있다. 예를 들어, 버튼을 클릭하면 화면이 업데이트되거나, 네트워크 요청 결과에 따라 내용이 변경될 수 있 이런 동적인 변화를 관리하기 위해서는 상태를 사용된다.

- **사용자 인터랙션:** 사용자의 입력에 따라 앱의 상태가 변경되어야 하며, 버튼 클릭, 텍스트 입력, 스크롤 등의 사용자 인터랙션에 따라 UI와 앱의 동작을 제어해야 한다.

- **데이터 표시:** 앱은 데이터를 표시하고 관리하며, 데이터의 변화에 따라 UI가 업데이트되어 사용자에게 항상 최신 정보를 제공해야한다.

- **애니메이션:** 애니메이션은 UI의 일부를 부드럽게 변경시키는 데 사용된다.

- **실시간 업데이트:** 앱은 외부 요인에 의해 실시간으로 업데이트되어야 할 때가 있다.(실시간 채팅, 실시간 주식 가격 변동 등)

### **State종류**

>**Ephemeral State (일시적인 상태):**

**Ephemeral state**는 일시적으로 유지되어야 하는 상태를 나타내며, 주로 StatefulWidget 내부에서 setState를 사용하여 관리된다. 예를 들어, 사용자의 입력에 따라 변경되는 폼 필드 값이나 버튼 활성화 여부와 같은 상태가 이에 해당되며, 이러한 상태는 해당 위젯의 생명 주기 동안만 유지되며, 위젯이 다시 빌드될 때 초기화된다.

>**App State (앱 상태):**

**App state**는 앱 전체에서 공유되는 상태를 나타내며, 여러 위젯이나 화면 사이에서 공유되어야 하는 데이터를 관리하는 데 사용된다.(로그인 상태, 앱의 테마 설정, 사용자 데이터와 같은 전역적인 상태)

<br>


# **Flutter Layout**

### **주요 위젯**

- **Container:** 박스 모양의 위젯으로, 크기와 배경, 테두리 등을 설정하여 내부의 다른 위젯을 감쌀 수 있음

- **Row:** 가로로 일렬로 위젯들을 배치

- **Column:** 세로로 일렬로 위젯들을 배치

- **Stack:** 위젯을 겹치게 배치하는 데 사용

- **Expanded:** 자식 위젯이 사용 가능한 모든 공간을 차지할 수 있도록 확장(Row나 Column 안에서 사용하여 공간을 분배)

- **Flexible:** 비율에 따라 공간을 할당하는데 사용(Row나 Column 안에서 사용되며, 공간을 유연하게 분할)

- **ListView:** 스크롤 가능한 목록 형태의 레이아웃을 만들 때 사용

- **GridView:** 그리드 형태의 레이아웃을 만들 때 사용


### **Animation 구현**

```dart
import 'dart:math';
import 'package:flutter/material.dart';

void main() => runApp(const AnimatedContainerApp());

class AnimatedContainerApp extends StatefulWidget {
  const AnimatedContainerApp({super.key});

  @override
  State<AnimatedContainerApp> createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('animatedContainer'),
        ),
        body: Center(
          child: AnimatedContainer(
            width: _width,
            height: _height,
            decoration: BoxDecoration(
              color: _color,
              borderRadius: _borderRadius,
            ),
            duration: const Duration(seconds: 1),
            curve: Curves.fastOutSlowIn,
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            setState(
              () {
                final random = Random();

                _width = random.nextInt(300).toDouble();
                _height = random.nextInt(300).toDouble();

                _color = Color.fromRGBO(
                  random.nextInt(256),
                  random.nextInt(256),
                  random.nextInt(256),
                  1,
                );

                _borderRadius =
                    BorderRadius.circular(random.nextInt(100).toDouble());
              },
            );
          },
          child: const Icon(Icons.play_arrow),
        ),
      ),
    );
  }
}

```


### **Navigation & Routing - 1**

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class Song {
  String name;

  Song({required this.name});
}

class _MyHomePageState extends State<MyHomePage> {
  final List<Song> songs = [
    Song(name: 'Bohemian Rhapsody'),
    Song(name: 'Imagine'),
    Song(name: 'What a Wonderful World'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Flutter Demo'),
      ),
      body: ListView.builder(
        itemCount: songs.length,
        itemBuilder: (context, index) {
          final song = songs[index];
          return ListTile(
            title: Text(song.name),
            onTap: () {
              Navigator.of(context).push(
                MaterialPageRoute(
                builder:(context) => SongScreen(song:song)
                )
              );
            },
          );
        },
      ),
    );
  }
}


class SongScreen extends StatelessWidget {
  final Song song;
  
  const SongScreen({super.key, required this.song});
  
  @override
  Widget build(BuildContext context){
    return Scaffold(
      appBar: AppBar(
      title: Text(song.name),
      ),
      body: Center(
        child: Text(song.name),
      ),
    );
  }
}

```


###  **Navigation & Routing - 2**

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Named Route Example',
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Screen'),
      ),
      body: Center(
        child: TextButton(
          onPressed: () {
            // Navigate to the DetailScreen using the `/details` route.
            Navigator.pushNamed(context, '/details');
          },
          child: Text('Go to Detail Screen'),
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Detail Screen'),
      ),
      body: Center(
        child: Text('This is the Detail Screen.'),
      ),
    );
  }
}
```