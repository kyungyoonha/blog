---
id: react_8
title: ※ Immer 라이브러리
sidebar_label: ※ Immer 라이브러리
---

## 1. 장점

-   불변성을 쉽게 유지하게 해준다.
-   nested data의 경우에 불변성을 유지하면서 변경하기 용이하다.

```JSX
// 적용 후
case POST_SET_LIKE: {
    const { likesOfMe, postSeq } = action.payload;
    const index = state.findIndex(post => post.seq === postSeq);
    const curLike = draft[index].likes;
    draft[index].likesOfMe = likesOfMe;
    draft[index].likes = likesOfMe ? curLike + 1 : curLike - 1;
    return draft;
}

// 적용 전
case POST_SET_LIKE: {
  const { likkesOfMe, postSeq } = action.payload;
  const index = state.findIndex(post => post.seq === postSeq);
  const curLike = state[index].likes;

  return state.map(post =>
    post.seq === postSeq
      ? {
          ...post,
          likesOfMe,
          likes: likkesOfMe ? curLike + 1 : curLike - 1,
        }
      : post
  );
}
```
