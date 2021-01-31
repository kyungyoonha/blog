---
id: devpedia_2
title: ※ 엑세스 토큰 만료시, 리프레시 토큰으로 엑세스 토큰 얻기
sidebar_label: ※ 엑세스 토큰 만료시, 리프레시 토큰으로 엑세스 토큰 얻기
---



## 1. 사용목적
- access Token 이 만료되었을 시, refreshToken을 이용하여 access token을 재발급 받는다.

## 2. 코드
- 401에러가 발생하였을 경우 엑세스 토큰을 재발급 받는 api를 날린다
  (다른 경우에도 401에러가 올 수 있으니, 서버에서 엑세스 토큰이 만료되었을때 오는 에러 메시지로 체크하는 것이 좋다. )
- 새로 엑세스 토큰을 발급 받은 경우 로컬스토리지에 저장 하고 api 헤더에 설정되어 있는 헤더를 변경해준다.
- 리프레시 토큰이 문제가 있을 경우에는 로그인 화면으로 이동(여기선 로그인이 모달로 구현되어 있으므로 로그인 모달 창을 띄어준다.)

```js
api.interceptors.response.use(
    function (response) {
        return response;
    },
    function (error) {
        if (error.response.data.status === 401 && error.response) {
            // 리프레시 토큰 오류
            const requestUrl = error.response.config.url;
            if (requestUrl === "/auth/token") {
                localStorage.clear();
                return store.dispatch(modalActions.setModal("login"));
            }

            // 엑세스 토큰 재발급
            const option = { headers: { RefreshToken: _refreshToken } };
            api.post("/auth/token", {}, option)
                .then((response) => {
                    const newToken = response.headers.authorization;
                    localStorage.setItem(
                        "accessToken",
                        JSON.stringify(newToken)
                    );
                    api.defaults.headers.common["Authorization"] = newToken;
                })
                // 리프레시 토큰 오류
                .catch((err) => {
                    return store.dispatch(modalActions.setModal("login"));
                });
        }

        return Promise.reject(error);
    }
);
```