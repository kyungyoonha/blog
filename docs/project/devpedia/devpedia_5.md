---
id: devpedia_5
title: ※ Card List Slick
sidebar_label: ※ Card List Slick
---

## 1. 동작
### 1-1 메인 페이지
|![gif](https://user-images.githubusercontent.com/29701385/106605948-d8b95a80-65a4-11eb-89ce-14d653a3d461.gif)|
|---|

### 1-2 디테일 페이지
|![gif](https://user-images.githubusercontent.com/29701385/106606798-e28f8d80-65a5-11eb-88c5-bd0f18d847ac.gif)|
|---|
|![gif](https://user-images.githubusercontent.com/29701385/106606809-e4f1e780-65a5-11eb-9053-e4677e999923.gif)|

## 2. 문제 점

- 왓차 피디아라는 사이트를 처음 분석할 때 슬라이드형 컴포넌트가 여러 페이지에 많이 쓰이는 것을 보고 모든 페이지에서 공통으로 쓸 수 있는 컴포넌트를 만들어보겠다는 생각하였고 이 때문에 페이지마다 기능이 다른 슬라이드가 있음에도 불구하고 한 컴포넌트에 많은 기능을 넣게 되었다. 따라서 코드도 길어지고 넘겨주는 props 가 많아졌다. 다시보니 페이지 별로 나누어 만들었으면 좀더 깔끔한 컴포넌트가 만들어지지 않았을까 하는 생각이 든다.
- 최근 제로초 강의에서 100줄 이상 넘으면 분리하는 것이 좋다는 강의를 보고나니 더 길어보인다.

## 3. 리팩토링
- 인물형 slick 컴포넌트 분리
- 전체 데이터를 넘겨주고 CardListSlick 안에서 destructure하여 데이터 보여주기
- css 관련 props는 전부 styled-component로 분리

## 4. 원리
- children으로 그려지는 내부 컴포넌트의 전체 width를 이용하여 이동할 수 있는 width(leftWidth)를 구한다.
- 오른쪽 버튼을 누를시 화면에 보여지는 컴포넌트 width(curWidth)만큼 이동
- 남은 width(leftWidth)가 화면에 보여지는 width(curwidth)보다 작은 경우 제일 오른쪽으로 이동
- 버튼 클릭시 api가 불러와지는 경우 fetchMore 여부에 따라서 페이지 이동을 결정한다.

## 4. 코드
```js
// css는 제외
const CardListSlick = ({
    title,
    description,
    posterUrl,
    count,
    sizeHeader,
    horizon,
    userId,
    fetchMore,
    onClickfetch = () => {},
    addComponent: AddComponent,
    children,
}) => {
    const slider = useRef();
    const [scollCtrl, setScrollCtrl] = useState({
        leftWidth: 0,
        slideWidth: 0,
    });
    const [buttonCtrl, setButtonCtrl] = useState({
        posX: 0,
        left: false,
        right: true,
        show: true,
    });

    useEffect(() => {
        try {
            const childNum = slider.current.children.length;
            const childWidth = slider.current.children[0].clientWidth;
            const slideWidth = slider.current.offsetWidth;
            const curWidth = buttonCtrl.posX + slideWidth;
            // 인물형 슬라이드의 경우 children 안에 카드 컴포넌트를 3줄로 보여주므로
            // 전체 width도 3분의 1로 나눠준다.
            const childTotalWidth = horizon
                ? Math.ceil(childNum / 3) * childWidth
                : childNum * childWidth;

            // 현재 넘긴 width를 제외하고 남은 width
            let leftWidth = childTotalWidth - curWidth;
            setScrollCtrl({ leftWidth, slideWidth });
            setButtonCtrl((state) => ({
                ...state,
                right: fetchMore || 50 < leftWidth,
            }));
        } catch (err) {
            console.log(err);
        }
    }, [buttonCtrl.posX, horizon, fetchMore]);

    const handleClickButton = (type) => {
        const { leftWidth, slideWidth } = scollCtrl;

        let newPosX;
        if (type === "right") {
            if (leftWidth > slideWidth || fetchMore) {
                newPosX = buttonCtrl.posX + slideWidth;
            } else newPosX = buttonCtrl.posX + leftWidth;
        } else {
            newPosX = buttonCtrl.posX <= slideWidth
                ? 0
                : buttonCtrl.posX - slideWidth;
        }
        setButtonCtrl({
            posX: newPosX,
            left: newPosX > 0,
            right: fetchMore || 50 < leftWidth - slideWidth,
        });
    };

    const handleMouseOver = () => {
        if (!buttonCtrl.show) {
            setButtonCtrl({ ...buttonCtrl, show: true });
        }
    };

    return (
        <Wrapper>
            <Header size={sizeHeader}>
                {userId ? (
                    <div className="title-block">
                        <a
                            className="user-info"
                            href={`/users/${userId}`}
                            alt=""
                        >
                            <img className="user-img" src={posterUrl} alt="" />
                            <div className="infoWrapper">
                                <p>{title}</p>
                                <div className="titleBlock">
                                    <div className="title">{description}</div>
                                </div>
                            </div>
                        </a>
                        <div className="titleRight">{AddComponent}</div>
                    </div>
                ) : (
                    <div className="titleBlock">
                        <div className="title">
                            {title}
                            <span>{count}</span>
                        </div>

                        <div className="titleRight">{AddComponent}</div>
                    </div>
                )}
            </Header>
            <Content
                onMouseOver={handleMouseOver}
                onMouseLeave={() =>
                    setButtonCtrl({ ...buttonCtrl, show: false })
                }
            >
                <div className="slickWrapper">
                    <div
                        className={`slickBlock ${horizon && "horizon"}`}
                        ref={slider}
                        style={{
                            transform: `translateX(-${buttonCtrl.posX}px)`,
                        }}
                    >
                        {children}
                    </div>
                    <ArrowButton
                        type="left"
                        show={buttonCtrl.show && buttonCtrl.left}
                        onClick={() => handleClickButton("left")}
                    />
                    <ArrowButton
                        type="right"
                        show={buttonCtrl.show && buttonCtrl.right}
                        onClick={() => {
                            onClickfetch();
                            handleClickButton("right");
                        }}
                    />
                </div>
            </Content>
        </Wrapper>
    );
};

export default CardListSlick;
```