---
id: css_1
title: Transform
sidebar_label: Transform
---

## 1 Transform

### 1- Rotate

-   회전

```CSS
.className { transform: rotate(20deg); transition: transform: .5s}
.className.active { transform: rotate(0);}
```

### 1-2 Scale

-   확대 축소
-   70% 축소 상태였다가 원래 상태로 변환

```CSS
.className { transform: scale(.7); transition: transform: .5s}
.className.active { transform: scale(1);}
```

### 1-3 Skew

-   기울이기

```CSS
.className { transform: skew(30deg); transition: transform: .5s}
.className.active { transform: skew(0);}
```

### 1-4 Translate

-   이동

```CSS
.className { transform: translate(100px, - 100px) transition: transform: .5s}
.className.active { transform: translate(0px 0px);}
```
