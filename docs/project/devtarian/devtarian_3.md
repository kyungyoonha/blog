---
id: devtarian_3
title: ※ 지도 검색으로 가장 가까운 가게 정보 가져오기
sidebar_label: ※ 지도 검색으로 가장 가까운 가게 정보 가져오기
---

## 1. 키워드로 위도, 경도 알아내기
- 키워드 검색하면 kakao Place API를 이용하여 새로 얻은 위도 경도를 알아낸다
- 새로운 값으로 URL queryString 정보를 변경해준다.
```js
import React, { useState } from 'react';
import styled from 'styled-components';
import history from '../../../history';
import queryString from 'query-string';
import { changeObjectToQuery } from '../../../utils/helper';

const SearchForm = () => {
  const [input, setInputs] = useState('종각역');
  const { q, ...query } = queryString.parse(history.location.search);
  
  const handleInputChange = (e) => {
    setInputs(e.target.value);
  };

  const handleInputClick = (e) => {
    e.preventDefault();
    var ps = new window.kakao.maps.services.Places();
    ps.keywordSearch(input, (data, status, pagination) => {
      if (status === window.kakao.maps.services.Status.OK) {
        
        window.location = `/search${changeObjectToQuery({
          ...query,
          q: input,
          lat: data[0].y,
          lng: data[0].x,
        })}`;
      } else {
        window.location = `/search?q=${input}&lat=37.573&lng=126.9794&range=0`;
      }
    });
    setInputs('');
  };

  return (
    <Wrap>
      <SearchInput
        placeholder={q ? q + ' 검색결과' : '근처의 채식 식당을 찾아보세요!'}
        value={input}
        data-show="show"
        onChange={handleInputChange}></SearchInput>
      <SearchBtn type="submit" data-show="show" onClick={handleInputClick}>
        검색
      </SearchBtn>
    </Wrap>
  );
};

export default SearchForm;

const Wrap = styled.form`
  position: relative;
  height: 49px;
`;

export const SearchInput = styled.input`
  z-index: 1;
  position: absolute;
  float: left;
  width: 100%;
  padding: 14px 10px;
  border-radius: 4px;
  background-color: ${(props) => props.theme.background[0]};
  border: 1px solid ${(props) => props.theme.gray[0]};
  box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px, rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
`;

const SearchBtn = styled.button`
  z-index: 1;
  position: absolute;
  right: 0;
  float: right;
  width: 50px;
  height: 103%;
  border-top-right-radius: 4px;
  border-bottom-right-radius: 4px;
  background-color: ${(props) => props.theme.green[1]};
  color: ${(props) => props.theme.background[0]};

  &:hover {
    background-color: ${(props) => props.theme.green[0]};
  }
`;

```
## 2. 서버: 위도 경도로 가장 근처 데이터 찾기 (geoFireStore)
```js
exports.getSearch = async (req, res) => {
    try {
        const q = req.query.q;
        const lat = req.query.lat;
        const lng = req.query.lng;
        const category = req.query.category || "all";
        const vegType = req.query.vegType || "all";
        const page = req.query.page || 1;
        const limit = req.query.limit || 5; 
        const order = req.query.order || "distance";
        const range = req.query.range ? Number(req.query.range) : 6; // 반경 몇 km로 검색할 것인지 설정

        if (!q) {
            return res.status(400).json({ error: "위치를 검색해주세요." });
        }
        const latitude = Number(lat);
        const longitude = Number(lng);
        const geocollection = new GeoFirestore(db).collection("store");
        const storeSnap = await geocollection
            .near({
                radius: range, // km
                center: new firebase.firestore.GeoPoint(latitude, longitude), // center 값으로 검색한 위도 경도를 넣어준다.
            })
            .get();

        let store = await Promise.all(
            storeSnap.docs.map(async (doc) => {
                let { g, ...rest } = doc.data();
                // 원하는 데이터 형태로 변경 후에 내려줌
                return {
                    ...rest,
                    id: doc.id,
                    distance: computeDistance({ latitude, longitude }, g.geopoint), // 거리 얼마나 떨어져있나
                    favorite: await checkFavorite(req, "storeId", doc.id), // favorite 유무 체크
                    imgUrl: rest.info.imgUrls[0] ? rest.info.imgUrls[0] : "", // 이미지는 대표이미지 하나만 내려줌
                };
            })
        );

        // 필터 category
        if (category !== "all") {
            store = store.filter((item) => item.info.category === category);
        }
        // 필터 vegType
        if (vegType !== "all") {
            store = store.filter((item) => item.info.vegType === vegType);
        }

        // 정렬
        order === "rated"
            ? store.sort(
                  (a, b) =>
                      Number(b.info.starRating) - Number(a.info.starRating)
              )
            : store.sort((a, b) => Number(a.distance) - Number(b.distance));


        // pagination
        store = store.splice((page - 1) * limit, limit);

        return res.status(200).json(store);
    } catch (err) {
        console.log(err);
        return res.status(500).json({
            error: err,
        });
    }
};
```

```js
// 거리 계산
module.exports = computeDistance = (startCoords, destCoords) => {
    var startLatRads = degreesToRadians(startCoords.latitude);
    var startLongRads = degreesToRadians(startCoords.longitude);
    var destLatRads = degreesToRadians(destCoords.latitude);
    var destLongRads = degreesToRadians(destCoords.longitude);

    var Radius = 6371; //지구의 반경(km)
    var distance =
        Math.acos(
            Math.sin(startLatRads) * Math.sin(destLatRads) +
                Math.cos(startLatRads) *
                    Math.cos(destLatRads) *
                    Math.cos(startLongRads - destLongRads)
        ) * Radius;

    return distance.toFixed(2);
};

const degreesToRadians = (degrees) => {
    radians = (degrees * Math.PI) / 180;
    return radians;
};

```

### 2-1 주의사항
- store정보 저장할때 다음과 같은 형식으로 저장해주어야한다.
|![devtarian_3_1](https://user-images.githubusercontent.com/29701385/107144019-a8463780-697b-11eb-96b7-7baa98090065.png)|
|---|

```js
exports.createStore = async (req, res) => {
    try {
        const newStore = {};
        const { lat, lng, ...info } = req.body;
        const { createdAt, email, ...user } = req.user;
        newStore.writer = user;
        newStore.coordinates = new admin.firestore.GeoPoint(lat, lng);
        newStore.info = info;
        newStore.createdAt = new Date().toISOString();
        newStore.reviews = 0;
        newStore.favorite = false;

        const geoFirestore = new GeoFirestore(db);
        const collection = geoFirestore.collection("store");

        await collection.add(newStore);
        return res.status(200).json({});
    } catch (err) {
        console.log(err);
        return res.status(500).json({ error: err });
    }
};
```