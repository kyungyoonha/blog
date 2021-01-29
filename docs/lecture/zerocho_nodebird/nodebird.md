## Mysql2
- 일종의 드라이버로 노드와 mysql을 연결시켜준다.

## 시퀄라이즈
- 자바스크립트로 sql을 조작할 수 있음
- npx sequelize init
- 한글 + 이모티콘 저장 -> utfmb4
```js
{
    charset: 'utf8md4',
    collate: 'utf8md4_general_ci'
}
```
- db.sequelize.sync() => 노드랑 sequelize를 연결

### 1-1. 노드에서 시퀄라이즈 사용
- modal 명은 


## db 설계
- 단일: comment / hashtag / image / post / user (기본)
- 모델이 두개이상 관련된 경우
    - user + post => 사용자가 쓴 게시물
    - user + comment => 사용자가 쓴 댓글 
    - post + hashtag => 게시글에 달린 해쉬태그

- 같은 테이블 끼리 관계가 있는 경우
    - user + user => 사용자가 사용자를 follow
- 일대 다 관계
    - user - post => 유저는 포스트를 여러개 쓸 수 있음
    - post는 오직 하나의 user만 가짐
    - user 테이블 - db.User.hasMany(db.Post);
    - post 테이블 - db.Post.belongsTo(db.User);

```js
User.associate = db => {
    db.User.hasMany(db.Post);
    db.User.hasMany(db.Comment);
    db.User.belongsToMany(db.Post, { through: 'Like', as: 'Liked'}); // 다대 다 => 좋아요 
    db.User.belongsToMany(db.User, { through: 'Follow' as 'Followers', foreignKey: 'fowllowingId' })
    // 나를 팔로윙하고 있는 사람을 찾으려면 followingId를 먼저 찾고 그 다음 팔로워들을 찾아야하므로
    // foreignKey => followingId
    // 즉 현재 내 key가 followingId 가 되고 이걸 이용해 followers를 찾는다
    db.User.belongsToMany(db.User, { through: 'Follow' as 'Followings', foreignKey: 'fowllowerId' })
    // 내가 팔로윙하고 있는 사람을 찾으려면 내 followerId를 먼저 찾고 그 다음 팔로윙을 찾아야하므로
    // foreignKey = follwerId
    // 즉 현재 내 key가 follwerId가 되고 이걸 이용해 followings를 찾음
}

Post.associate =db => {
    db.Post.belongsTo(db.User);
    db.Post.belongsToMany(db.Hashtag) // 다대 다 관계
    db.Post.hasMany(db.Comment);
    db.Post.hasMany(db.Image)
    db.Post.belongsToMany(db.User, { through: 'Like', as 'Likers' }) // 다대 다 => 좋아요
    db.Post.belongsTo(db.Post, { as: 'Retweet' }) // 리트윗 하나의 원본 트윗은 여러개의 리트윗을 가질 수 있음
    
}

Comment.associate = db => {
    db.Comment.belongsTo(db.User);
    db.Comment.belongsTo(db.Post)
}

Image.associate = db => {
    db.Image.belongsTo(db.Post);
}

Hashtag.associate db => {
    db.hashtag.belongsToMany(db.Post)
    // 해쉬태그는 태그 하나가 여러개의 게시글도 가지면서
    // 게시글이 여러개의 태그를 가질 수 있음
}
```

## belongsTo vs hasMany
- hasMany는 딱히 기능이 없음
- belongsTo는 연결된 모델의 id값을 가진 가상의 컬럼이 생성된다
- db.Comment.belongsTo(db.User) => Comment 모델에 UserId 라는 컬럼 생김
- db.Comment.belongsTo(db.Post) => Comment 모델에 PostId라는 컬럼 생김

## 일대일 관계 hasOne
- User - UserInfo 
- 사용자와 그 사용자의 데이터는 일대일 관계임
- 오직 하나로만 연결되어 있다.

## 다대다 관계 belongsToMany
![](https://user-images.githubusercontent.com/29701385/106023858-8bec0480-610a-11eb-9e8e-efa1ef0c6166.png)
- 어쩔 수 없이 중간 테이블이 하나 생성된다
- 중간 테이블이 있어야 검색이 가능해지므로
- through를 통해 중간 테이블의 이름을 변경할 수 있다.
    - 예를들어 User 와 Post라면 default로 UserPost가 생김
    - 좋아요라고 명확하게 알수 없고 헷갈리므로 { through: 'like'}

- as 사용하는 이유
    - as를 이용하여 속해있는 데이터 한번에 가져올 수 있음
    - belongsTo로 자동으로 생기는 id의 이름을 변경할 수 있다.
    - db.Post.belongsTo(db.Post, { as: 'Retweet' })
    - Post 모델 안에 PostId 컬럼이 생기면 이상하므로 RetweetId로 변경 
    - ![](https://user-images.githubusercontent.com/29701385/106028257-10d91d00-610f-11eb-8f4e-238e9a6cc0d7.png)


- 같은 테이블끼리 다대 다 가능(유저가 유저 팔로우)
    - 역시나 중간 테이블이 생긴다.

## foreignKey 
- 다대 다 관계에서 테이블 명이 다른 경우는 컬럼명이 UserId / PostId 처럼 구분이 됌
- 같은 테이블에서 다대 다 관계인 경우는 중간테이블의 컬럼 명이 같을 수 없으므로 foreignKey를 이용하여
변경해주어야한다.

## bycrypt
- 비밀번호 암호화할때사용
- 옵션은 12 -> 너무 길면 오래걸림
- 1초정도 걸리는 숫자로 정해주는 것이 좋다.
- 비동기 이므로 async 써줘야한다.

## 에러는 next로 한방에 처리
```js
router.post('/', async (req, res, next) => {
    try{

    }catch(err){
        console.log(err)
        next(err)
    }
})

```

## CORS
- 브라우저에서 다른 도메인 서버로 요청했을 경우 발생
- 프론트 서버에서 백엔드 서버로 보낼때는 cors가 안생긴다 -> 브라우저에서 서버로 요청시에만 발생
- 해결. 서버에서 헤더에 Access-Control-Allow-Origin을 설정해준다.
- 해결. 모든 요청에 대해서 허용해버리면 해커가 서버로 바로 데이터 전송할 수 있으므로 특정 도메인만 설정해준다.
```js
 // 방법1: 라우터에서 설정
 res.setHeader('Access-Control-Allow-Origin', '*') // http://localhost:3000
 res.status(200).send('ok')

// 방법 2: cors()
app.use(cors({
    origin: 'http://nodebird.com',
    credential: true // default 값은 false임 => 
}))
```

- 프록시 방식
    - 브라우저(3000)에서 같은 도메인의 프론트 서버(3000)로 요청 -> 프론트서버(3000)에서 다른도메인의 백엔드 서버(3060)로 요청
    - 서버에서 서버로 전송하는건 문제 없음
    - 응답할때도 백엔드 서버에서 프론트서버로 먼저보내고 브라우저로 보낸다  


## passport
- 로그인 관련된것을 한번에 처리


### 미들웨어 확장
```js
// 적용전
// next()를 쓸수가 없다.
router.post('/login', passport.authenticate('local', (err, user, info) => {
    if(err){
        console.error(err);
        next(err)
    }
})

// 적용후 
router.post('/login', (req, res, next) => {
    passport.authenticate('local', (err, user, info) => {
        if(err){
            console.error(err);
            next(err)
        }
    })(req, res, next);
})
```

## passport + 쿠키와 세션
- 쿠키 & 세션 원리
    - 로그인을 하면 브라우저랑 서버가 같은 정보를 가지고 있어야하는데 브라우저는 누구나 접속가능하므로 해킹에 취약하다
    - 유저정보를 브라우저에 보낼 수 없으므로 (비밀번호 등) 랜덤한 문자열을 쿠키에 담아서 보내준다.
    - 서버는 랜덤한 문자열과 유저정보를 묶어서 세션으로 가지고 있는다.
    - 브라우저가 api 요청할때 쿠키에 랜덤한 문자열을 가지고 있으면 서버 세션에서 랜덤 문자열로 원래 유저 정보로 복원하여 누군지 알 수 있음
- passport
    - passport 사용하면 쿠키에 랜덤한 문자열 자동으로 담아준다. (res.setHeader('Cookie', 'fewfwcewcw))
    - 서버는 모든 유저정보를 다 가지고 있는 것이 아니라 랜덤한 문자열과 매칭되는 id 값만 가지고 있음
    - 유저 정보는 시간이 지날수록 무거워지므로 데이터를 다 가지고 있으면 메모리가 심하게 낭비(댓글수, 결제정보 등등)
    - req.login(user)
        - serializeUser() => 유저 정보중에서 쿠키의 랜덤 문자열과 묶어줄 id값만 남겨준다.
        - deserializeUser() => id값으로 유저정보 찾아서 복원한다.

- 나중에는 세션 저장용 db로 redis를 사용한다.

## Route.replace()
- 뒤로가기 했을때 그 페이지의 기록을 지우고 싶은 경우 사용한다.