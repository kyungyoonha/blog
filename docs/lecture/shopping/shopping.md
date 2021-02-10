mongoDb. inclued는 populate()로 가져온다.
mongoDb. offset은 skip( )으로

## 금액 필터
- 범위 필터일 경우 아래와 같이 array로 컨트롤 하면 편함
```js
const data = [
    {
        "_id": 0,
        "name": "Any",
        "array": []
    },
    {
        "_id": 2,
        "name": "$0 to $199",
        "array": [0, 199]
    },
    {
        "_id": 0,
        "name": "$200 to $399",
        "array": [200, 300]
    }
]
```

## 몽고DB - Search 가중치
- 검색할때 title에 5만큼의 비중, description에는 1만큼의 비중을 주겠다.
```js
ProductSchema.index({
    title: 'text',
    description: 'text',
}, {
    weights: {
        title: 5,
        description: 1
    }
})
```

## 몽고DB - 업데이트: 숫자 count 올리기 / 추가
```js
// 새로운 상품이 이미 존재하는 경우
if(duplicate){
    User.fineOneAndUpdate(
        {_id: req.user.+_id, "card_id": req.body.productId },
        { $inc : { "cart.$.quantity" : 1 }}, // $inc 는 increment의 약자, 원하는 데이터의 count를 증가 시킬 수 있다.
        { new: true }, // true설정할시 return되는 값을 새로 업데이트 되는 값으로 받아온다. false하면 기존 값으로 내려옴
        (err, userInfo) => {
            if(err) return res.status(200).json({ success: false, err })
            res.status(200).send(UserInfo.cart) 
        }
    )
}
// 새로운 상품이 없을 경우, 신규추가
else{
    UserfineOneAndUpdate(
        {_id: req.user._id},
        {
            $push:{
                cart: { // cart데이터 안에 push된다.
                    id: req.body.productId, 
                    quantity: 1,
                    date: Date.now()
                }
            },
            {new: true},
            (err,UserInfo) => {
                if(err) return res.status(200).json({ success: false, err })
                res.status(200).send(UserInfo.cart)
            }
        }
    )
}
```