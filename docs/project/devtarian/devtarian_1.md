---
id: devtarian_1
title: ※ Firebase로 노드 서버 배포하기
sidebar_label: ※ Firebase로 노드 서버 배포하기
---

## 1. 프로젝트
이번 프로젝트에서는 프론트와 서버를 모두 Firebase를 이용하여 배포하였으며 서버는 function를 이용하여 api와 트리거를 만들어주었다.

## 2 Functions: 서버 배포

### 2-1 설치 및 세팅

```js
$ firebase init // 시작
$ firebase deploy // 배포
```
### 2-2 Firebase Admin SDK
```JS

// firebase 홈페이지 > 프로젝트 개요 > 프로젝트 설정 > 서비스 계정  
// serviceAccount 정보를 받아서 아래에 SERVICE_ACCOUNT 넣어주면된다.
const config = require("./config");
const SERVICE_ACCOUNT = require("./admin.json");
const firebase = require("firebase");

const admin = require("firebase-admin");

firebase.initializeApp(config);
admin.initializeApp({
    credential: admin.credential.cert(SERVICE_ACCOUNT), 
    databaseURL: "https://project-devtarian-default-rtdb.firebaseio.com/",
});

const db = admin.firestore();

module.exports = { firebase, admin, db };
```
### 2-3 코드
- 서비스지역은 asia-northeast3(한국)으로 설정해준다.
```JS
const functions = require("firebase-functions");
const app = require("express")();
const bodyParser = require("body-parser");
const cors = require("cors");
const { db } = require("./fbAdmin");

// middleware
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cors());

const swaggerJSDoc = require("swagger-jsdoc");
const swaggerUi = require("swagger-ui-express");
const swaggerSpec = swaggerJSDoc(require("./swagger"));

app.use("/swagger-doc", swaggerUi.serve, swaggerUi.setup(swaggerSpec));
app.use(require("./routes"));

exports.api = functions.region("asia-northeast3").https.onRequest(app);
```




