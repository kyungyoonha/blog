---
id: css_3
title: ※ BEM
sidebar_label: ※ BEM
---

## 1 BEM란?

-   Block + Element + Modifier

## 2. 사용예제

```CSS
.card                  /* Block*/
.card--dark            /* Block + Modifier*/
.card--red             /* Block + Modifier*/
.card__picture         /* Block + Element*/
.card__title           /* Block + Element*/
.card__list            /* Block + Element*/
.card__item            /* Block + Element*/
.card__button          /* Block + Element */
.card__button--active  /* Blcok + Element + Modifier */
/* 이름은 하이픈 하나로 구분 */
.search-form           /* Block*/
.search-form__content  /* Block + Element*/
.serach-form__input    /* Block + Element*/
.search-form__button   /* Block + Element*/
```

## 3. styled-components에서 사용

```js
const Wrapper = styled.div`
    .card {
        width: 100%;
        height: 100vh;

        &__list {
            display: flex;
            background: yellow;
        }

        &__item {
            flex: 1;
        }
    }
`;
```

## 4 주의사항

### 4.1 Element 끼리는 동일한 레벨로 취급하여 하나의 Element만 작성한다

```CSS
(x) .card__list__item
.card__list
.card__item
```

### 4-2 Block이 여러개 인경우

-   Block안에 Block이 있는 경우 그냥 새롭게 작성해주면된다.
-   form이라는 Block안에 Nav Block이있음

```js
<div className="form">
    <div className="form__header">
        <div className="nav">
            <ul className="nav__list">
                <li className="nav__item">홈</li>
                <li className="nav__item">안내</li>
                <li className="nav__item">지도</li>
                <li className="nav__item">소개</li>
            </ul>
        </div>
    </div>
    <div className="form__body"></div>
    <div className="form__footer"></div>
<div>
```

### 4-3 modefier active

```js
import React from "react";
import styled from "styled-components";

const Test = () => {
    const [isClicked, setIsClicked] = useState(false);

    const onClickButton = () => {
        setIsClicked(!isClicked);
    };
    return (
        <Wrapper>
            <div
                className={`test__title ${
                    isClicked ? "test__title--active" : ""
                }`}
            >
                {isClicked ? "클릭됌" : "클릭안됌"}
            </div>
            <button
                className="test__button"
                type="button"
                onClick={onClickButton}
            >
                버튼
            </button>
        </Wrapper>
    );
};

export default Test;

const Wrapper = styled.div`
    .test__title {
        font-size: 3rem;
        text-align: center;

        &--active {
            color: red;
            font-weight: 900;
        }
    }
`;
```
