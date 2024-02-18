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





### 썸네일 추출 & 썸네일 URL 업데이트

```typescript

//index.ts

import * as functions from "firebase-functions";
import * as admin from "firebase-admin";

admin.initializeApp();

export const onVideoCreated = functions.firestore
.document("videos/{videoId}")
.onCreate(async(snapshot,context) => {
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
    const [file, _] = await storage.bucket().upload(`/tmp/${snapshot.id}.jpg`, {
        destination: `thumbnails/${snapshot.id}.jpg`,
    });
    await file.makePublic();
    await snapshot.ref.update({thumbnailUrl: file.publicUrl()});
    const db = admin.firestore();

    await db.collection("users")
    .doc(video.creatorUid)
    .collection("videos")
    .doc(snapshot.id)
    .set({
        thumbnailUrl: file.publicUrl(), 
        videoId: snapshot.id});
});
```

*"우선, Cloud Functions 설치를 완료하면 index.ts라는 typescript 파일이 생성되고, 위처럼 해당 파일에 Firebase Cloud Functions와 Firebase Admin SDK를 사용하여 Firestore에서 특정 이벤트가 발생할 때 자동으로 실행되는 백엔드 로직을 구현한다."*

<br>
<br>

## "썸네일 추출 & 썸네일 URL 업데이트" 코드 뜯어보기

### 1. Firebase Functions와 Admin SDK 불러오기

```typescript
import * as functions from "firebase-functions";
import * as admin from "firebase-admin";
```

- `import * as functions` 구문은 firebase-functions 모듈에서 제공하는 모든 기능을 functions라는 이름의 객체로 가져오며, functions.firestore, functions.auth 등과 같이 Cloud Functions에서 필요한 다양한 이벤트 처리기에 접근한다.

- `import * as admin` 구문은 firebase-admin 패키지에서 제공하는 모든 기능을 admin이라는 객체로 가져오며, admin.firestore(), admin.auth() 등 Firebase 서비스에 서버 측에서 접근한다.



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

export const onVideoCreated = functions.firestore
.document("videos/{videoId}")
.onCreate(async(snapshot,context) => {
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
    const [file, _] = await storage.bucket().upload(`/tmp/${snapshot.id}.jpg`, {
        destination: `thumbnails/${snapshot.id}.jpg`,
    });
    await file.makePublic();
    await snapshot.ref.update({thumbnailUrl: file.publicUrl()});
    const db = admin.firestore();

    await db.collection("users")
    .doc(video.creatorUid)
    .collection("videos")
    .doc(snapshot.id)
    .set({
        thumbnailUrl: file.publicUrl(), 
        videoId: snapshot.id});
});
```

### 1. `const spawn = require('child-process-promise').spawn;`

- Node.js의 `child_process` 모듈의 `spawn` 함수를 프로미스(Promise) 기반으로 사용할 수 있게 해주는 `child-process-promise` 라이브러리를 불러온다. `spawn` 함수는 새로운 프로세스를 생성하여 주어진 명령어를 실행시키는 데 사용되며, 여기서는 `ffmpeg` 명령어를 실행하는 데 사용된다.

### 2. `const video = snapshot.data();`

- `snapshot.data()`는 Firestore 문서의 데이터를 가져온다. 위 경우, `video` 변수는 Firestore에서 가져온 현재 문서(비디오 정보를 담고 있는)의 데이터를 담게 된다.

### 3. `await spawn("ffmpeg",[...]`

- `await` 키워드는 `spawn` 함수의 실행이 완료될 때까지 함수 실행을 일시 정지하고, `ffmpeg` 프로세스의 완료를 기다린다. 이는 `ffmpeg` 작업이 비동기적으로 처리되며, 작업 완료 후 다음 코드로 진행하기 위해 사용된다.

### 4. `ffmpeg` 명령어의 옵션들:

- `-i video.videoUrl`: 입력 파일로 `video.videoUrl`에서 지정한 비디오 파일을 사용한다.(실제 비디오 파일 위치를 가리킴)

- `-ss 00:00:01.000`: 비디오의 시작부터 1초 지점을 썸네일 이미지 생성을 위한 시작 시간으로 설정한다.

- `-vframes 1`: 비디오에서 단 하나의 프레임만 캡처하여 이미지를 생성한다.(하나의 썸네일 이미지만 필요하므로)

- `-vf "scale=150:-1"`: 추출된 이미지의 스케일을 조정한다. 여기서는 너비를 150픽셀로 지정하고, 높이는 원본 비율(-1)을 유지하여 자동으로 조정되게 한다.

- `/tmp/${snapshot.id}.jpg`: 생성된 썸네일 이미지 파일의 저장 위치와 파일명을 지정한다. 임시 디렉토리(`/tmp`)에, Firestore 문서 ID(`snapshot.id`)를 기반으로 한 파일명으로 저장된다.


### 5. `await file.makePublic();`

- `file.makePublic()` 함수를 호출하여, Cloud Storage에 저장된 썸네일 이미지 파일을 공개적으로 접근 가능하게 설정한다.(인증 없이도 해당 파일의 URL을 통해 누구나 이미지에 접근할 수 있도록 변경)

### 6. `await snapshot.ref.update({thumbnailUrl: file.publicUrl()});`

- `snapshot.ref.update()` 함수를 사용하여, 처음에 Firestore에 생성된 비디오 문서를 업데이트하고, `thumbnailUrl` 필드에 `file.publicUrl()`을 통해 얻은 썸네일 이미지의 공개 URL을 저장한다.

### 7. `const db = admin.firestore();`

- `admin.firestore()`를 호출하여 Firestore 데이터베이스의 인스턴스를 가져오고, 인스턴스를 통해 Firestore 데이터베이스에 접근하여, 문서나 컬렉션에 대한 작업을 수행할 수 있다.

### 8. Firestore에 사용자 비디오 정보 업데이트

- Firestore의 `users` 컬렉션 내에 특정 사용자(`video.creatorUid`)의 문서 안에 `videos`라는 서브 컬렉션을 참조하고, 해당 서브 컬렉션 내에 새로운 문서를 생성하거나 업데이트한다.

- 문서의 ID는 썸네일 이미지와 동일한 `snapshot.id`를 사용한다.

- `.set()` 함수를 사용하여 문서에 `{ thumbnailUrl: file.publicUrl(), videoId: snapshot.id }` 객체를 저장한다.

- 이 과정을 통해, 사용자의 `videos` 서브 컬렉션에 새로운 비디오의 썸네일 URL과 ID 정보가 저장되며, 이는 사용자가 자신의 비디오를 관리하거나 애플리케이션에서 비디오 목록을 표시할 때 사용될 수 있다.