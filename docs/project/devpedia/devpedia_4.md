---
id: devpedia_4
title: ※ 무한스크롤 1. Intersection Observer
sidebar_label: ※ 무한스크롤 1. Intersection Observer
---

## 1. 동작
### 1-1 content/{contentId}
|![img](https://user-images.githubusercontent.com/29701385/106480721-0a6fea00-64ef-11eb-9b79-b0afcfb621c8.gif)|
|---|
### 1-2 detail/decks/{pageId}
|![img](https://user-images.githubusercontent.com/29701385/106479745-ff688a00-64ed-11eb-8eeb-dbe07cf769a7.gif)|
|---|

### 1-3 /users/{userId}/movies/ratings
|![img](https://user-images.githubusercontent.com/29701385/106481522-dd700700-64ef-11eb-89a1-a184cf0c443c.gif)|
|---|
## 2. 설명
- isFetching = true 일때 페이지 스크롤 제일 하단의 <Loader> 컴포넌트가 보이면 추가로 데이터를 가지고 오는 API를 호출한다.
- 더 가지고 올 데이터가 없는 경우 fetchMore = false

## 3. 코드
### 3-1 useObserver.js
- 초기 fetch시 isObserve = true로 변경되고 observer 시작한다.
- entry.intersectionRatio > 0 일경우 isIntersecting state를 true로 변경해준다. 
```js 
import { useState, useEffect, useRef } from "react";

const options = {
    root: null,
    rootMargin: "0px",
    threshold: 0.25,
};
const useObserver = (isObserve) => {
    const ref = useRef();
    const [isIntersecting, setIntersecting] = useState(false);

    useEffect(() => {
        const observer = new IntersectionObserver(([entry]) => {
            isObserve && entry.intersectionRatio > 0
                ? setIntersecting(true)
                : setIntersecting(false);
        }, options);
        const elem = ref?.current;
        if (elem) {
            observer.observe(elem);
        }
        return () => {
            observer.unobserve(elem);
        };
    }, [ref, isObserve]);

    return [ref, isIntersecting];
};

export default useObserver;

```

### 3-2 CardListInfinite.js
- 하단 <Loader>에 ref를 걸어준다.
- 더보기 눌렀을 때 & 초기 패치 완료 후 & fetchMore = true 일때 데이터를 더 가지고 온다.
```js
// style 생략
const CardListInfinite = ({ posters, fetchUrl }) => {
    const SIZE = 12;
    const [showMore, setShowMore] = useState(true);

    const dispatch = useDispatch();
    const detail = useSelector((state) => state.detail);
    const { data, initFetch, isFetching, fetchMore } = detail;
    const [ref, isIntersecting] = useObserver(initFetch);

    const handleClick = () => {
        setShowMore(false);
    };

    useEffect(() => {
        dispatch(detailActions.init(posters, SIZE));
        return () => dispatch(detailActions.initialize());
    }, [dispatch, posters]);

    useEffect(() => {
        if (isIntersecting && !showMore && fetchMore) {
            dispatch(detailActions.fetchMore(fetchUrl));
        }
    }, [dispatch, isIntersecting, showMore, fetchMore, fetchUrl]);

    return (
        <Wrppaer>
            <CardList title="작품들">
                {data.map((item, idx) => (
                    <StyledCard
                        key={idx}
                        item={item}
                        onClick={() =>
                            (window.location = `/contents/${item.id}`)
                        }
                    />
                ))}
            </CardList>
            {showMore && <Button onClick={handleClick}>더보기</Button>}
            <StyledLoader ref={ref}>
                {isFetching && <Loader height="800px" />}
            </StyledLoader>
        </Wrppaer>
    );
};
```
### 3-3 detailActions.js
```js
const init = (data, size) => async (dispatch) => {
    try {
        dispatch({
            type: DETAIL_INIT,
            payload: { data, size },
        });
    } catch (err) {
        console.error(err.response ? err.response : err);
    }
};

const fetchMore = (fetchUrl) => async (dispatch, getState) => {
    try {
        dispatch({ type: DETAIL_FETCHING });
        
        // getState()로 현재 데이터 가지고옴
        let { page, size } = getState().detail;
        // 현재 url 뒤에 query String을 붙여준다.
        // http://localhost:3000?size=10&page=10
        const res = await api.get(
            makeUrlQuery(fetchUrl, { size, page: page + 1 })
        );
        dispatch({
            type: DETAIL_FETCH_DATA,
            payload: res.data,
        });
    } catch (err) {
        console.error(err.response ? err.response : err);
    }
};
```

### 3-4 detailReducers.js
```js
const detailReducers = (state = INITIAL_STATE, action = {}) => {
    switch (action.type) {
        // 초기 fetch 후에 initFetch를 true로 변경
        case DETAIL_INIT:
            return {
                ...state,
                ...action.payload,
                page: state.page + 1,
                initFetch: true,
            };
        // 패치될때마다 page를 1씩 증가
        // 요청하는 데이터 개수보다 더 적은 수의 data가 오는 경우 fetchMore을 false로 변경
        case DETAIL_FETCH_DATA:
            return {
                ...state,
                data: [...state.data, ...action.payload],
                page: state.page + 1,
                isFetching: false,
                fetchMore: action.payload.length < state.size ? false : true,
            };

        case DETAIL_INITIALIZE:
            return INITIAL_STATE;

        case DETAIL_FETCHING:
            return {
                ...state,
                isFetching: true,
            };
        default:
            return state;
    }
};
```