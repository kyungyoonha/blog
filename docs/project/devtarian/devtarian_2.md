---
id: devtarian_2
title: ※ Firebase functions로 트리거 만들기
sidebar_label: ※ Firebase functions로 트리거 만들기
---

## 1. 설명
- firebase에서 문서 삭제시에 관련되어 있는 다른 데이터들을 같이 한번에 지울 수 없다
- 문서를 보니 수동 삭제해주어야 한다고 나와있다.
- 데이터들을 다 찾아서 지워줘야 하는데 생각보다 많은 데이터를 찾아서 지워야 하므로 서버가 느려지는 것을 막고자 functions에 onDelete 트리거를 이용하여 구현해보았다. 

![devtarian_2](https://user-images.githubusercontent.com/29701385/107137426-ceee7900-694f-11eb-966d-65585c4727fe.png)|
|---|

## 2. 코드
- 가게 / wiki 정보가 삭제될 경우 onDelete 트리거가 작동하여 관련된 데이터를 삭제한다.
- batch()를 이용하여 한번에 실행되도록 한다.
```js
exports.onStoreDelete = functions
    .region("asia-northeast3")
    .firestore.document("/store/{storeId}")
    .onDelete(async (snapshot, context) => {
        try {
            const storeId = context.params.storeId;
            const batch = db.batch();

            const reviewDoc = await db
                .collection("review")
                .where("storeId", "==", storeId)
                .get();

            reviewDoc.docs.forEach((doc) => {
                batch.delete(db.doc(`/review/${doc.id}`));
            });

            const likeDoc = await db
                .collection("like")
                .where("storeId", "==", storeId)
                .get();

            likeDoc.docs.forEach((doc) => {
                batch.delete(db.doc(`/like/${doc.id}`));
            });

            const favoriteDoc = await db
                .collection("favorite")
                .where("storeId", "==", storeId)
                .get();

            favoriteDoc.docs.forEach((doc) => {
                batch.delete(db.doc(`/favorite/${doc.id}`));
            });

            const commentDoc = await db
                .collection("comment")
                .where("storeId", "==", storeId)
                .get();

            commentDoc.docs.forEach((doc) => {
                batch.delete(db.doc(`/comment/${doc.id}`));
            });

            batch.commit();
        } catch (err) {
            console.log(err);
        }
    });
```

```js
exports.onWikiDelete = functions
    .region("asia-northeast3")
    .firestore.document("/wiki/{wikiId}")
    .onDelete(async (snapshot, context) => {
        try {
            const wikiId = context.params.wikiId;
            const batch = db.batch();

            const favoriteDoc = await db
                .collection("favorite")
                .where("wikiId", "==", wikiId)
                .get();

            favoriteDoc.docs.forEach((doc) => {
                batch.delete(db.doc(`/favorite/${doc.id}`));
            });

            const commentDoc = await db
                .collection("comment")
                .where("wikiId", "==", wikiId)
                .get();

            commentDoc.docs.forEach((doc) => {
                batch.delete(db.doc(`/comment/${doc.id}`));
            });

            batch.commit();
        } catch (err) {
            console.log(err);
        }
    });
```

|![devtarian_2_1](https://user-images.githubusercontent.com/29701385/107137668-2c83c500-6952-11eb-8435-a30bb30ea7a3.png)|
|---|
