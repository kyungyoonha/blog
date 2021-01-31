---
id: devpedia_1
title: ※ Custom Hooks로 Form inputs 관리하기
sidebar_label: ※ Custom Hooks로 Form inputs 관리하기
---

## 1. 🎞 동작
![](https://user-images.githubusercontent.com/29701385/106356341-4a459e80-6342-11eb-8b97-f2f1eb117dfe.gif)

## 2. 🍳사용 목적
- 라이브러리 도움 없이 Form Inputs 관리
- 타이핑을 할 때마다 Input Validate 체크
- Custom 에러 메시지
- 제출시 필수 항목 체크 및 에러 메세지
- 컴포넌트 최적화 - 수정하고 있는 inputs 을 제외하고 다른 컴포넌트들은 리렌더링 되지 않도록 한다.

## 3. ⚙ 방법

####  3-1 Hooks/useInputs.js
- onChange, onSumit, onSubmitFile 함수는 props로 넘겨줘야 하므로 useCallback을 사용하여 최적화 하였다. 
- file 은 formData에 담아서 서버에 전달해야하므로 onSumitFile을 따로 만들어주었다.

```jsx
const useInputs = (initialValue) => {
    const [inputs, setInputs] = useState(initialValue);
    const [errors, setErrors] = useState([]);

    const onChange = useCallback((e) => {
        const { name, value, type, files } = e.target;
        
        type === "file"
            ? setInputs((state) => ({ ...state, [name]: files[0] }));
            : setInputs((state) => ({ ...state, [name]: value }));

        const error = validate(name, value);
        setErrors((state) => ({ ...state, [name]: error }));

    }, []);

    const onSubmit = useCallback(async (pathname, data) => {
        try {
            let { isValid, checkedErrors } = validateAll(data);
            if (!isValid) return setErrors(checkedErrors);

            await api.post(pathname, data);
        } catch (err) {
            console.error(err.response);
        }
    }, []);

    // 파일이 포함되어 있는 경우
    const onSubmitFile = useCallback(async (pathname, data, reqFileName) => {
        try {
            let { isValid, checkedErrors } = validateAll(data);
            if (!isValid) return setErrors(checkedErrors);

            let { file, ...body } = data;
            let formData = new FormData();

            formData.append(reqFileName, file);
            formData.append("body", new Blob([JSON.stringify(body)], { type: "application/json" }));

            await api.post(pathname, formData);
        } catch (err) {
            console.error(err.response);
        }
    }, []);

    return { inputs, setInputs, errors, setErrors, onChange, onSubmit, onSubmitFile };
};
```
#### 3-2 utils/validate.js
- switch case문으로 name값 마다 원하는 에러 메시지 출력
- 정규식 이용
```js
export const validate = (name, value) => {
    
    switch (name) {
        case "name":
            return value.length <= 1 && "이름을 입력해주세요";

        case "password":
            return value.length <= 3 && "비밀번호를 3자리 이상 입력해주세요";
        case "email":
            let regExp = /^[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/i;
            return !regExp.test(value) && "이메일 양식에 맞게 작성해주세요.";

        case "bookRate":
        case "page":
        case "runningTimeInMinutes":
        case "totalAudience":
            return checkNumber(value) && "숫자만 입력 가능합니다.";

        default:
            return;
    }
};

export const checkNumber = (input) => {
    const regexp = /^[0-9]*$/;
    return !regexp.test(input) ? true : false;
};
```

#### 3-3 사용예시
- useInputs() 값중에서 필요한 값만 destructure해서 사용

```js
const PageParticipant = () => {
    const { inputs, errors, onChange, onSubmitFile } = useInputs(initialValue);

    const handleSubmit = () => {
        onSubmitFile("/admin/participants", inputs, "profile");
    };

    return (
        <FormSection title="인물 등록">
            <div className="row">
                <div className="col-4 d-flex flex-column">
                    <File name="file" value={inputs.file} onChange={onChange} />
                </div>
                <div className="col-8">
                    <Input
                        title="이름"
                        name="name"
                        value={inputs.name}
                        onChange={onChange}
                        error={errors.name}
                    />
                    <Input
                        title="직업"
                        name="job"
                        value={inputs.job}
                        onChange={onChange}
                        error={errors.job}
                    />
                    <Textarea
                        title="설명"
                        name="description"
                        value={inputs.description}
                        onChange={onChange}
                        error={errors.description}
                        rows="3"
                    />
                </div>
            </div>
            <button
                type="button"
                className="btn btn-primary"
                onClick={handleSubmit}
            >
                Submit
            </button>
        </FormSection>
    );
};
```

#### 3-4 Inputs.js 
- 최적화를 위해 React.memo() 사용
- props가 바뀌지 않는한 리렌더링 되지 않는다.
- props 중에서 함수는 useCallback으로 감싸줘야 리렌더링 되지 않는다.
```js
import React from "react";

const Input = ({ 
    className, title, name, value, onChange, error, disabled, children,
}) => {
    return (
        <div className={`form-group ${className}`}>
            <label>{title}</label>
            <div className="input-group">
                <input
                    name={name}
                    value={value}
                    onChange={onChange}
                    className={`form-control ${error && "is-invalid"}`}
                    disabled={disabled}
                />
                {children && (
                    <div className="input-group-append">
                        <span className="input-group-text">{children}</span>
                    </div>
                )}

                {error && <div className="invalid-feedback">{error}</div>}
            </div>
        </div>
    );
};

export default React.memo(Input);
```

#### 3-5 제출 버튼 클릭시
- validateAll() 함수로 한번더 validate 체크하고 빈값이 있으면 에러메시지 출력해준다.
```js
export const validateAll = (inputs) => {
    let isValid = false;
    let checkedErrors = {};

    Object.keys(inputs).forEach((key) => {
        if (key !== "description" && key !== "job") {
            let errorMessage = validate(key, inputs[key]);
            if (errorMessage) {
                checkedErrors[key] = errorMessage;
            }
        }
    });

    if (Object.keys(checkedErrors).length === 0) {
        isValid = true;
    }

    return { isValid, checkedErrors };
};
```


## 4. 개선할 점
- 위에 코드는 <form> 태그 안에 작성하지 않았는데 <form> 태그로 감싸주고 제출버튼의 타입을 sumit으로 해주면 엔터를 눌러도 제출이 가능하다.
- 제출시 빈값을 체크하기 위해 validateAll()을 만들어서 관리하고 있는데 그냥 input 속성중에서 required를 넣어주는 것이 더 간편할 것 같다.
- form 데이터는 굳이 리덕스로 전역관리할 필요가 없을 것 같다는 생각이 들었으나 redux로 관리하면 서버에서 내려오는 validate 에러 또한 쉽게 컴포넌트로 내려줄 수 있을것 같다 redux-form이라는 라이브러리 documemnt도 살펴 봐야겠다.
