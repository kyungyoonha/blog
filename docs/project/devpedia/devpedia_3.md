---
id: devpedia_3
title: ※ 마우스 따라다니는 평점 별 컴포넌트 만들기
sidebar_label: ※ 마우스 따라다니는 평점 별 컴포넌트 만들기
---

## 1. 🎞 동작
![동작](/img/project/devpedia/stars.gif)

## 2. 원리.
- 색이 없는 별 5개를 밑에 깔아주고 노란색의 별 컴포넌트는 그 위에 겹치게하고 마우스 움직임에 따라서 width가 조정되게 한다.
- width: 0% => 별 0 개
- width: 40% => 별 2개  
- width: 50% => 별 2.5개
- width: 100% => 별 5개
## 3. 코드
#### 3-1 star.js

```js
const Star = ({ isRated }) => {
    return isRated ? <StarRated/> : <StarNormal/>
};


const StarRated = styled.div`
    display: inline-block;
    background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0NCIgaGVpZ2h0PSI0NCIgdmlld0JveD0iMCAwIDQ0IDQ0Ij4KICAgIDxnIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCI+CiAgICAgICAgPHBhdGggZmlsbD0iI0ZGREQ2MyIgZD0iTTIyIDMzLjQ0NEw5LjgzIDQyLjMyN2MtLjc4NC41NzItMS44NDItLjE5Ni0xLjUzOS0xLjExOGw0LjY4Ny0xNC4zMkwuNzY5IDE4LjA2Yy0uNzg3LS41NjktLjM4My0xLjgxMi41ODgtMS44MWwxNS4wNjcuMDMzIDQuNjI0LTE0LjM0Yy4yOTgtLjkyNCAxLjYwNi0uOTI0IDEuOTA0IDBsNC42MjQgMTQuMzQgMTUuMDY3LS4wMzNjLjk3MS0uMDAyIDEuMzc1IDEuMjQxLjU4OCAxLjgxbC0xMi4yMDkgOC44MjkgNC42ODggMTQuMzJjLjMwMi45MjItLjc1NiAxLjY5LTEuNTQgMS4xMThMMjIgMzMuNDQ0eiIvPgogICAgPC9nPgo8L3N2Zz4K)
        center center / contain no-repeat;
    vertical-align: top;
    width: 40px;
    height: 40px;
`;

const StarNormal = styled.div`
    display: inline-block;
    background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0NCIgaGVpZ2h0PSI0NCIgdmlld0JveD0iMCAwIDQ0IDQ0Ij4KICAgIDxnIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCI+CiAgICAgICAgPHBhdGggZmlsbD0iI0VFRSIgZD0iTTIyIDMzLjQ0NEw5LjgzIDQyLjMyN2MtLjc4NC41NzItMS44NDItLjE5Ni0xLjUzOS0xLjExOGw0LjY4Ny0xNC4zMkwuNzY5IDE4LjA2Yy0uNzg3LS41NjktLjM4My0xLjgxMi41ODgtMS44MWwxNS4wNjcuMDMzIDQuNjI0LTE0LjM0Yy4yOTgtLjkyNCAxLjYwNi0uOTI0IDEuOTA0IDBsNC42MjQgMTQuMzQgMTUuMDY3LS4wMzNjLjk3MS0uMDAyIDEuMzc1IDEuMjQxLjU4OCAxLjgxbC0xMi4yMDkgOC44MjkgNC42ODggMTQuMzJjLjMwMi45MjItLjc1NiAxLjY5LTEuNTQgMS4xMThMMjIgMzMuNDQ0eiIvPgogICAgPC9nPgo8L3N2Zz4K)
        center center / contain no-repeat;
    vertical-align: top;
    width: 40px;
    height: 40px;
`;
```

#### 3-2 stars.js 
- 단위 변환
    - 별 하나의 길이는 40px 이므로 총 길이는 200px
    - 181 ~ 200 => 5
    - 161 ~ 180 => 4.5
    - ...
    - 1 ~ 20 => 0.5
    - 0 => 0 
```js
const Stars = () => {
    const dispatch = useDispatch();
    const [mouseRate, setMouseRate] = useState(0);
    const {
        userData: { score },
    } = useSelector((state) => state.content);

    // 초기 Score 가 있는 경우 초기 값 변경
    useEffect(() => {
        setMouseRate(score);
    }, [score]);

    // 단위 변환
    const handleMouseMove = (e) => {
        setMouseRate((Math.floor((e.nativeEvent.layerX / 40) * 2) + 1) / 2);
    };

    const handleMouseLeave = () => {
        setMouseRate(score || 0);
    };

    const handleClick = async () => {
        return mouseRate === score
            ? dispatch(contentActions.deleteStar()) // 같은값 클릭시 기존값 제거
            : dispatch(contentActions.setStar(mouseRate));
    };

    return (
        <StarContainer rate={mouseRate}>
            <div className="text">{ratedTextMap[score] || "평가하기"}</div>

            <div
                className="star_block"
                onMouseMove={handleMouseMove}
                onMouseLeave={handleMouseLeave}
                onClick={handleClick}
            >
                {[...new Array(5)].map((_, idx) => (
                    <Star key={idx} />
                ))}
                <div className="rated_stars">
                    {[...new Array(5)].map((_, idx) => (
                        <Star key={idx} isRated={true} />
                    ))}
                </div>
            </div>
        </StarContainer>
    );
};

const StarContainer = styled.div`
    .text {
        color: rgb(120, 120, 120);
        font-size: 12px;
        font-weight: 400;
        letter-spacing: -0.2px;
        line-height: 16px;
        text-align: center;

    }

    .star_block {
        position: relative;
        text-align: center;
        width: 200px;
        margin: 0px auto;
        cursor: pointer;

        display: inline-block;
        position: relative;
        vertical-align: top;
        height: 40px;
    }

    .unratedStars {
        display: inline-block;
        position: relative;
        vertical-align: top;
        height: 40px;
    }

    .rated_stars {
        position: absolute;
        top: 0px;
        left: 0px;
        white-space: nowrap;
        width: ${(props) => (props.rate ? props.rate * 20 + "%" : "0")};
        height: 40px;
        overflow: hidden;
    }

`;
```