---
layout: post
title: "[Flutter] 의존성 주입(Dependency Injection)"
date: 2023-09-07
categories: [Flutter]
tags: [flutter]
---



### 글로벌 인스턴스 사용:

*글로벌 인스턴스 사용은 애플리케이션 전반에서 단일 공유 인스턴스에 대한 접근을 제공했다.*

#### 특징

- **접근성**: 어디에서나 직접 접근이 가능했다. 예를 들어, `FirebaseFirestore.instance`는 Firestore 서비스의 글로벌 인스턴스를 나타내며, 앱의 어느 곳에서나 바로 사용할 수 있었다.

- **간단함**: 복잡한 설정이나 초기화 없이 바로 사용할 수 있어, 코드가 단순하고 이해하기 쉬웠다.

- **유연성 부족**: 특정 인스턴스에 강하게 결합되어 있어서, 다른 인스턴스로의 전환(예: 테스트 환경으로의 전환)이 어려웠다.

- **테스트 어려움**: 글로벌 상태에 의존하기 때문에 테스트 시에 모의 객체(Mocking)를 사용하기 어려웠다.


### 의존성 주입:

*의존성 주입은 객체가 필요로 하는 의존성(서비스나 다른 객체)을 외부로부터 받아오는 디자인 패턴이었다.*

#### 특징

- **분리 및 재사용성**: 의존성이 외부에서 제공되므로, 코드 간 분리가 잘 되어 있고 재사용성이 높았다.

- **테스트 용이성**: 테스트 시에 필요한 의존성을 쉽게 모의 객체로 대체할 수 있어, 단위 테스트(Unit Testing)가 용이했다.

- **유연성**: 다양한 환경(개발, 스테이징, 프로덕션)에 대해 다른 설정이나 인스턴스를 쉽게 전환할 수 있었다.

- **복잡성 증가**: 의존성을 관리하기 위한 추가적인 코드나 프레임워크가 필요하며, 설계가 복잡해질 수 있었다.

<br>

**글로벌 인스턴스 사용 예시**:
```dart
class UserRepository {
  User getUser() {
    // 글로벌 인스턴스 직접 사용
    return GlobalDatabase.instance.getUser();
  }
}
```

**의존성 주입 사용 예시**:
```dart
class UserRepository {
  final Database database;

  UserRepository(this.database);

  User getUser() {
    // 주입된 의존성 사용
    return database.getUser();
  }
}
```

글로벌 인스턴스 사용은 빠르게 시작하기 좋지만, 프로젝트가 성장하고 복잡해질수록 의존성 주입의 장점이 두드러지고, 의존성 주입을 통해 코드를 더 유연하고 관리하기 쉽게 만들 수 있다.



### 의존성 주입을 사용하지 않는 경우:

```dart
class WeatherService {
  String getWeather() {
    // API를 호출하여 날씨 정보를 가져옴
    return "Sunny";
  }
}

class WeatherReport {
  WeatherService _weatherService = WeatherService();

  String getReport() {
    return "Today's weather is ${_weatherService.getWeather()}";
  }
}
```

위 코드에서 `WeatherReport` 클래스는 직접적으로 `WeatherService` 클래스를 인스턴스화했다. 이 방식은 단순하고 빠르게 구현할 수 있지만, 몇 가지 문제가 있었다:

- `WeatherService` 클래스를 다른 서비스로 바꾸고 싶으면 `WeatherReport` 클래스의 코드를 수정해야 했다.

- `WeatherService`에 대한 단위 테스트를 할 때 실제 API 호출이 일어나면 테스트가 느려지고 외부 요인에 의존하게 되었다.

### 의존성 주입을 사용하는 경우:

```dart
abstract class IWeatherService {
  String getWeather();
}

class WeatherService implements IWeatherService {
  @override
  String getWeather() {
    // API를 호출하여 날씨 정보를 가져옴
    return "Sunny";
  }
}

class WeatherReport {
  IWeatherService _weatherService;

  WeatherReport(this._weatherService);

  String getReport() {
    return "Today's weather is ${_weatherService.getWeather()}";
  }
}

// 사용 예
void main() {
  IWeatherService service = WeatherService();
  WeatherReport report = WeatherReport(service);

  print(report.getReport());
}
```

위 코드에서는 `WeatherReport` 클래스가 추상화된 `IWeatherService`에 의존하고 있다. 이제 `WeatherReport` 클래스는 `IWeatherService` 인터페이스를 구현하는 어떤 클래스든 받아 사용할 수 있다. 이 방식은 다음과 같은 장점이 있다:

- `WeatherService`를 다른 구현체로 교체하고 싶을 때 `WeatherReport` 클래스의 코드 변경 없이 `main` 함수에서 다른 `IWeatherService` 구현체를 주입하면 된다.

- 테스트 시에는 실제 서비스 대신 모의 객체(Mock Object)를 `WeatherReport`에 주입하여 빠르고 일관된 테스트가 가능하다.

```dart
class MockWeatherService implements IWeatherService {
  @override
  String getWeather() {
    return "Mock Weather"; // 테스트용 가짜 날씨 정보
  }
}

// 테스트 예
void testWeatherReport() {
  IWeatherService mockService = MockWeatherService();
  WeatherReport report = WeatherReport(mockService);

  // 'getReport' 메소드를 테스트할 수 있습니다.
}
```

이 예에서 볼 수 있듯이, 의존성 주입을 사용하면 `WeatherReport` 클래스가 날씨 서비스의 구체적인 구현에 덜 종속되고, 유연성이 증가하여 다른 환경이나 요구사항에 더 쉽게 적응할 수 있다. 또한, 유닛 테스트가 용이해져서 더 안정적이고 관리하기 쉬운 코드를 작성할 수 있게 된다.


#### *`implements`?

`implements` 키워드는 Dart와 다른 객체 지향 프로그래밍 언어에서 인터페이스를 구현할 때 사용된다. 인터페이스는 특정 클래스가 따라야 할 메서드의 시그니처(이름, 반환 타입, 매개변수)를 정의하는 계약과 같은 것이다. 하지만 인터페이스 자체는 이 메서드들의 구체적인 구현을 포함하지 않는다.

클래스가 인터페이스를 구현(implement)한다고 선언할 때, 그 클래스는 인터페이스에 정의된 모든 메서드를 구현해야 하고, 이것은 해당 클래스가 인터페이스에 명시된 "형태"와 "기능"을 모두 갖추고 있음을 의미한다.

예를 들어, 다음과 같이 인터페이스를 정의하고 구현할 수 있다:

```dart
// 인터페이스 정의
abstract class Animal {
  void eat();
  void move();
}

// 인터페이스 구현
class Bird implements Animal {
  @override
  void eat() {
    // 먹는 행위에 대한 구현
  }

  @override
  void move() {
    // 움직임에 대한 구현
  }
}
```

위 예시에서 `Bird` 클래스는 `Animal` 인터페이스를 구현한다. 이것은 `Bird` 클래스가 `eat`과 `move` 메서드를 반드시 구현해야 함을 의미하는데. `@override` 키워드는 `Bird` 클래스가 인터페이스의 메서드를 오버라이드(재정의)하여 구현했음을 나타낸다.

`implements`를 사용하는 주된 이유는 다음과 같다:

1. **다형성**: 서로 다른 클래스들이 같은 인터페이스를 구현함으로써, 이들 클래스의 인스턴스를 인터페이스 타입으로 처리할 수 있다. 이는 다양한 구현체들을 같은 방식으로 사용할 수 있게 해주어 코드의 유연성을 높힌다.

2. **계약 준수**: 클래스가 인터페이스를 구현하면, 그 클래스가 인터페이스에 정의된 계약을 준수하게 되고 이는 해당 클래스가 특정 기능을 가지고 있음을 보장합니다.

3. **코드 재사용성 및 유지보수**: 여러 클래스가 동일한 인터페이스를 공유할 수 있기 때문에 코드의 재사용성이 향상되고, 인터페이스를 기반으로 코드를 유지보수하는 것이 더 쉬워진다.

`implements`를 사용하여 인터페이스를 구현함으로써, 클래스는 인터페이스에서 정의된 모든 메서드를 구현해야 하며, 이를 통해 인터페이스의 모든 기능을 제공해야 한다는 계약을 이행하게 된다.

<br>

### 간단한 의존성 주입을 연습할 수 있는 두 가지 예제:

#### 예제 1: 음악 플레이어 인터페이스와 의존성 주입

```dart
// 음악 플레이어의 인터페이스
abstract class MusicPlayer {
  void play(String songName);
}

// 음악 플레이어의 구현체
class MyMusicPlayer implements MusicPlayer {
  @override
  void play(String songName) {
    print("Playing $songName");
  }
}

// 음악 플레이 리스트를 관리하는 클래스
class Playlist {
  final MusicPlayer player;

  Playlist(this.player);

  void playAll() {
    player.play("Song 1");
    player.play("Song 2");
    // 다른 노래들을 재생
  }
}

void main() {
  MusicPlayer myPlayer = MyMusicPlayer();
  Playlist myPlaylist = Playlist(myPlayer);
  myPlaylist.playAll();
}
```

위 예제에서 `Playlist` 클래스는 `MusicPlayer` 인터페이스에 의존하고 있다. `main` 함수에서 실제 구현체인 `MyMusicPlayer`를 `Playlist`에 주입하고 있다. 이를 통해 플레이리스트가 특정 음악 플레이어 구현체에 종속되지 않는다.

#### 예제 2: 데이터 저장소 인터페이스와 의존성 주입

```dart
// 데이터 저장소의 인터페이스
abstract class DataRepository {
  String getData();
}

// 데이터 저장소의 구현체
class CloudDataRepository implements DataRepository {
  @override
  String getData() {
    return "Data from cloud";
  }
}

// 데이터 저장소의 또 다른 구현체
class LocalDataRepository implements DataRepository {
  @override
  String getData() {
    return "Data from local storage";
  }
}

// 데이터 소비자 클래스
class DataConsumer {
  final DataRepository repository;

  DataConsumer(this.repository);

  void printData() {
    print(repository.getData());
  }
}

void main() {
  DataRepository cloudRepository = CloudDataRepository();
  DataConsumer cloudConsumer = DataConsumer(cloudRepository);
  cloudConsumer.printData();
  
  DataRepository localRepository = LocalDataRepository();
  DataConsumer localConsumer = DataConsumer(localRepository);
  localConsumer.printData();
}
```

이 예제에서 `DataConsumer` 클래스는 `DataRepository` 인터페이스에 의존하고 있으며, `main` 함수에서 클라우드와 로컬 저장소를 나타내는 두 가지 구현체를 각각의 소비자에게 주입하고 있다. 이를 통해 데이터 소비 로직이 특정 저장소 구현에 종속되지 않으며, 다양한 저장소 전략을 쉽게 교체할 수 있다.

