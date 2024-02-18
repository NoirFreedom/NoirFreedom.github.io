---
layout: post
title: "[Firebase/Cloud Functions] Cloud Functions - Thumbnail 추출을 위한 코드 뜯어보기"
date: 2024-01-28
categories: [Firebase]
tags: [firebase, cloud functions]
---

#### *"코드 뜯어보기에 앞서, 사용된 Firebase 서비스 각각의 기능을 다시 한번 살펴보겠다."*

### Firestore
Firebase의 NoSQL 클라우드 데이터베이스로, 데이터를 저장하고 동기화하는 데 사용된다. 실시간 데이터베이스 기능을 제공하여, 앱의 데이터가 자동으로 업데이트되고, 오프라인에서도 작동할 수 있게 해준다. 주로 텍스트나 경량의 데이터 구조(예: 사용자 정보, 메시지 등)를 저장하는 데 적합하다.

### Firebase Storage
Firebase Storage는 큰 파일(이미지, 비디오, 오디오 등)을 저장하고 서비스하는 데 최적화된 서비스이고, Google Cloud Storage를 기반으로 하며, 사용자가 업로드하는 대용량 파일을 안전하게 저장하고 네트워크 상태와 관계없이 효율적으로 전송할 수 있도록 설계되었다.

### Cloud Functions
Cloud Functions는 서버리스 컴퓨팅 환경을 제공하여, 특정 이벤트가 발생했을 때 개발자가 미리 정의해 둔, 예를들어 이미지를 압축하거나, 워터마크를 추가하는 함수를 자동으로 실행시키며, Firestore의 데이터 변경, Firebase Authentication의 사용자 생성/삭제, Firebase Storage에 파일 업로드 등 다양한 이벤트를 트리거로 사용할 수 있다.




<br>
<br>

### 이미지 업로드 및 Cloud Functions
Firebase Storage에 이미지나 비디오 파일이 업로드되는 이벤트를 Cloud Functions를 사용하여 감지하고, 이를 트리거로 특정 함수를 실행시키는 것이 가능하다. 예를 들어, 이미지가 업로드될 때 자동으로 이미지를 압축하거나, 이미지에 워터마크를 추가하는 작업을 Cloud Functions를 통해 자동화할 수 있는데, 아래는 Storage에 영상이 업로드 되는 것을 감지하고, 영상에서 썸네일을 추출하는 로직이다.



#### 썸네일을 추출하는 로직

```typescript

//index.ts

import * as functions from "firebase-functions";
import * as admin from "firebase-admin";

admin.initializeApp();

export const onVideoCreated = functions.firestore.document("videos/{videoId}").onCreate(async(snapshot,context) => {
    const spawn = require('child-process-promise').spawn;
    const video = snapshot.data();
    await spawn("ffmpeg",[
        "-i",
        video.videoUrl,
        "-ss",
        "00:00:01.000",
        "-vframes",
        "1",
        "-vf",
        "scale=150:-1",
        `/tmp/${snapshot.id}.jpg`
    ]);
    const storage = admin.storage();
    await storage.bucket().upload(`/tmp/${snapshot.id}.jpg`, {
        destination: `thumbnails/${snapshot.id}.jpg`,
    });
});
```

*"우선, Cloud Functions 설치를 완료하면 index.ts라는 typescript 파일이 생성되고, 위 같이 해당 파일에 Firebase Cloud Functions와 Firebase Admin SDK를 사용하여 Firestore에서 특정 이벤트가 발생할 때 자동으로 실행되는 백엔드 로직을 구현한다."*

<br>
<br>

## 코드 뜯어보기

### 1. Firebase Functions와 Admin SDK 불러오기

```typescript
import * as functions from "firebase-functions";
import * as admin from "firebase-admin";
```

firebase-functions는 Cloud Functions for Firebase를 작성할 때 필요한 기능을 제공하는 라이브러리이다.
firebase-admin은 서버 측에서 Firebase의 다양한 서비스(예: Authentication, Firestore, Firebase Storage)에 접근할 수 있는 권한을 제공하는 SDK이다.



### 2. Firebase Admin SDK 초기화

```typescript
admin.initializeApp();
```

**환경 설정**:  
`initializeApp()` 함수는 Firebase 프로젝트의 구성 정보를 로드한다. 이 정보에는 데이터베이스 URL, 인증 키, 프로젝트 ID 등이 포함될 수 있고, 이 구성 정보를 통해 SDK는 올바른 Firebase 프로젝트와 연결되고, 해당 프로젝트의 서비스에 접근할 수 있게 된다.

**보안 인증**:  
서버 사이드 애플리케이션에서 Firebase 서비스에 접근하기 위해서는 적절한 인증 정보를 제공해야 하기에, initializeApp으로 인증 정보를 로드하고 사용하여 Firebase 서비스에 안전하게 접근할 수 있는 환경을 만든다.

**서비스 인스턴스 생성**:  
Firestore나 Firebase Storage 서비스에 접근하려면, 서비스의 인스턴스가 생성되어 있어야하며, initializeApp 함수 호출 후, admin.firestore(), admin.storage()와 같은 방식으로 각 서비스에 접근할 수 있다.





### 3. Cloud Function 정의

```typescript
export const onVideoCreated = functions.firestore.document("videos/{videoId}").onCreate(async (snapshot, context) => { ... });
```

- `functions.firestore.document("videos/{videoId}")`: Firestore의 `videos` 컬렉션에 있는 특정 문서(`{videoId}`를 통해 동적으로 문서 ID를 받음)에 대한 이벤트 리스너를 설정한다.

- `.onCreate(async (snapshot, context) => { ... })`: 새 문서가 생성될 때 실행될 콜백 함수를 정의한다. 비동기(`async`)함수로 선언되어, 문서의 데이터에 접근할 수 있는 `snapshot` 객체와 이벤트에 대한 추가 정보를 제공하는 `context` 객체를 매개변수로 받는다.



### 4. `child-process-promise`와 `ffmpeg` 사용

```typescript
const spawn = require('child-process-promise').spawn;
await spawn("ffmpeg", [ ... ]);
```

- `require('child-process-promise').spawn`: Node.js의 `child_process` 모듈의 `spawn` 함수를 프로미스 기반으로 사용할 수 있게 하는 라이브러리를 불러오며, 이를 통해 비동기적으로 외부 프로세스(여기서는 `ffmpeg`)를 실행시킬 수 있다.

- `ffmpeg` 명령을 사용하여 비디오 파일에서 첫 번째 프레임을 썸네일로 추출한다. 명령어 옵션들은 입력 파일(`-i`), 썸네일을 추출할 시간(`-ss`), 추출할 프레임 수(`-vframes`), 출력 이미지의 크기(`-vf` "scale=150:-1")를 지정하며, 추출된 이미지는 임시 디렉토리(`/tmp`)에 저장된다.



### 5. Firebase Storage에 썸네일 업로드

```typescript
const storage = admin.storage();
await storage.bucket().upload(`/tmp/${snapshot.id}.jpg`, {
    destination: `thumbnails/${snapshot.id}.jpg`,
});
```

- `admin.storage()`: Firebase Admin SDK를 통해 Firebase Storage의 인스턴스에 접근합니다.

- `storage.bucket().upload(...)`: 지정된 경로(`/tmp/${snapshot.id}.jpg`)에 있는 파일을 Firebase Storage의 `thumbnails` 폴더에 업로드하고, 업로드될 파일의 목적지는 `destination` 옵션을 통해 지정한다.

<br>

위 코드를 통해 Firestore에 비디오 문서가 추가될 때마다 자동으로 해당 비디오의 첫 프레임을 캡쳐하여 썸네일로 만들고, 이를 Storage에 업로드하는 과정을 자동화할 수 있으며, 이는 애플리케이션의 비디오 콘텐츠 관리를 효율적으로 할 수 있다.