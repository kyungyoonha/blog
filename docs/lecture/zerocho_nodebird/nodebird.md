## 제로초 강의

### 리덕스 사가
- request / success / failure 까지 정의해준다.
- throttle / takeLatest => 지연시켜주지만 둘다 리퀘스트는 발생한다, response만 한번 받음

### immer 
- 손쉽게 불변성을 유지시켜준다
- filter -> unshift
- 배열 합치기 -> concat

### 성능 최적화
- 프로젝트시 수천개의 데이터를 잘 처리해서 보여주는 것이 좋다.
- faker 이용


### facker
- 더미 데이터 사용하기 좋음
- 이미지까지

### 무한스크롤
```js
useEffect(() => {
    const scroll = () => {
        if (winodw.scrollY +  document.documentElement.clientHeight > document.documentElement.scrollHeight - 300){
            if(hasMorePost &&)
        }
        // 3가지만 있으면 됌
        // scollY(다 내렸을때) + clientHeight = scrollHeigth
        // scoll Y :얼마나 내렸는지
        // clientHieght: 보이는 화면 높이
        // scrollHeight: 총 길이
    }
    
    window.addEventListner('scroll', scroll)
    return () => window.removeEventListner('scroll', scroll)
}, [])
```

### 무한스크롤 scroll 이벤트시, requst 한번만 요청하기
- request -> isLoading : true
- success -> isLoading : false
- failure -> isLoading : false
- 즉, isLoading이 false일때만 추가로 불러오는 api를 요청하면 된다. 

### react-verturelize
- 너무많은 데이터를 가지고 오는 경우 메모리가 부족 -> 특히나 모바일
- 이런 경우 지나간 데이터는 메모리에만 있고 화면에서는 지워주고 새로운 데이터를 추가해준다.
- 인스타그램 같은 예시임
- 수백개가 이미 로딩되어 있고 화면에는 3개만 보인다 


### 서버와 프론트를 나누는 이유
- 대규모 앱이 되었을때는 나누는 것이 효과적이다.
- 메모리가 터지는 것을 방지하기 위해 스케일링을 해주는데 프론트랑 서버가 같이 구현되어있으면
- 스케일링 할때마다 전체가 다 복사되어 매우 비효율 적이다.
- 프론트는 서버사이드 렌더링, 백엔드는 API를 제공하는 역할을 하는데 한쪽에만 요청이 몰릴 수 있음
- 대규모 프로젝트에서는 기능별로 서버를 나눠준다 -> 특정 기능에 데이터 요청이 많이 왔을때 그 기능만 서버를 여러개로 늘려준다.
- 배민 같은 경우 피크타임때 주문용 서버, 결제용 서버만 여러개 늘려놓으면됌 -> 상품등록 /어드민은 늘릴필요가 없다.


### 노드는 서버가 아니다.
- 노드는 서버가 아니라 자바스크립트 런타임 환경임
- 따로 설치할 필요가 없고 http를 require하여 사용한다
- const http = require('http');

### REST API
- put -> 전체수정
- patch -> 부분수정
- options -> 찔러보기, 서버가 내 요청을 받아 줄 것인지 체크
- head -> 헤더만 가져오기

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

## 다대다 관계 belongsToMany 데이터 추가하기
```js
// 특정 유저를 내가 팔로우할때 그 유저의 팔로워에 내 아이디를 추가한다.
router.patch('/:userId/follow', isLoggedIn, async (req, res, next) => {
    try{
        const user = await User.findOne({ where: { id: req.params.userId }});
        if(!user){
            res.status(403).send('없는 사람을 팔로우 할 수 없습니다');
        }
        await user.addFollowers(req.user.id);
        // await user.removeFollowers(req.user.id);
        // await user.getFollowers()
        // await user.getFollowings()
        res.status(200).json({ nickname: req.body.nickname })
    }catch(err){
        console.error(err)
        next(err)
    }
})
```

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


## 정리
노드. delete시 postId 내려줌
노드. data 내려줄때 object 형식으로 내려준다
노드. res.status(200).json({ postId: postId }) => 프론트에서 res.data.postId로 접근하게 / res.data보다는 명확함


## 고차함수
- 고차함수란 함수를 인자로 받거나 또는 함수를 리턴함으로써 작동하는 함수를 말한다.
- map()문법도 고차함수이다.
```js
const onCancel  = (id) => () => {
    dispatch({
        type: UNFOLLOW_REQUEST
    })
}


return(
    <>
        { [...new Array(4)].map(_, idx) => {
            <Card onClick={onCancel(item.id)}></Card>
        }}
        
    </>
)
```

```js
const arr1 = [1,2,3];
arr1.map(item => {
    console.log(item)
})
```


## 이미지 업로드를 위한 multer
- 파일이나 이미지 비디오는 multipart 데이터로 올라간다.
- 프론트에서 백엔드로 데이터를 보낼때 json과 urlencoded 두개로만 받게되면
- multipart 데이터 형식은 백엔드에서 받을 수 없다.
- form으로 보낼경우 -> urlencoded
```js

app.use(express.json());
app.use(express.urlencoded({ extended: true }))
```
### multer 미들웨어
- 파일 저장을 컴퓨터의 하드디스크에 저장할 경우 안좋음
- 요청이 많아 서버 스케일링을 하게 될 경우(같은 서버를 여러개 복사) 파일도 같이 복사된다.
- 서버에 쓸데없는 공간을 계속 차지하게 되므로 클라우드로 분리하는게 좋다.
```js
const upload = multer({
    storage: multer.diskStore({
        destination(req, file, done){
            done(null, 'upload'); // 하드디스크 upload 폴더에 넣어놓겠다. -> 나중에는 aws 클라우드에 저장할 것임
        },
        filename(req, file, done){
            done(null)
        }
    })
})

// 사용

```

파일 업로드. 이미지 20MB로 제한 -> 서버 공격이 될 수 있다.
파일 업로드. 동영상 100MB ~ 200MB 
파일 업로드. 되도록 파일은 백엔드를 안거치고 프론트에서 바로 클라우드로 넘기는 것이 좋다.
파일 업로드. 이미지나 동영상 처리는 서버의 cpu와 메모리를 많이 잡아 먹는다.
파일 업로드. db에 파일 자체를 저장하는 것이 아니라 src 주소만 가지고 있는다.
파일 업로드. db에 파일 자체를 넣으면 db가 무거워지고 캐싱도 할 수 없으므로 파일은 따로 저장 
파일 업로드. 방식1. 이미지 + json. 이미지 데이터와 json 데이터를 같이 보내는 방법
파일 업로드. 방식1. 장점. api 통신은 한번이면 끝난다
파일 업로드. 방식1. 단점. 이미지 + json. 파일 리사이징이나 파일 미리보기가 애매해진다.
파일 업로드. 방식2. 이미지 먼저 업로드 -> json 데이터 업로드
파일 업로드. 방식2. 파일만 업로드하고 데이터 등록을 안하는 경우가 생길 수 있고 api가 여러번 통신됌
파일 업로드. 방식2. 하지만, 보통 이미지도 자산이기 때문에 서버에 남겨놓는 것이 좋다. 따로 삭제해주진 않음
파일 업로드. 방식2. 이미지가 차지하는 용량에 대한 비용보다 이미지를 얻음으로써 얻을 수 있는 가치가 더 크다.
파일 업로드. 방식2. 나중에는 이미지로 머신러닝도 돌릴 수 있음


multer. upload.array('image') => 이미지 여러개 처리
multer. upload.single('image') => 이미지 하나 처리
multer. upload.none() => 텍스트만 처리


### 유사배열
- input type="file" 인 경우 유사배열이 생김
- 개발자도구에서 __proto__를보면 forEach가 없음
```js
const onChangeImage = (e) => {
    const imageFormData = new FormData();
    [].forEach.call(e.target.files, (f) => {
        imageFormData.append('image', f)
    }
})
```

### path
- path.join으로 경로 합쳐주는이유는 운영체제에 따라서 / 또는 \ 로 경로가 바뀔 수 있다.

### static 
- app.use('/', express.static.(path.join(__dirname, 'upload' )))
- 그냥 '/'로만 해놓는게 좋다.
- 프론트에서 벡엔드의 파일 구조를 알수가 없으므로 보안에 더 유리함
- 프론트에서는 http://localhost/ 인데 실제는 upload파일 안에 있음

### requset / success / fail 3가지로 나눠서 만드는 이유
- 비동기 액션이므로 요청과 success / fail을 나눠서 처리해줘야한다.
- 동기 액션일 경우에는 하나만 만들어줘도 된다.
```js
// 업로드된 이미지 제거시 서버와 따로 통신할 필요없이 제거 api 날리고 동기로 바로 지워주면 된다 

case REMOVE_IMAGE:
    draft.imagePaths =draft.imagePaths.filter((v, i) => i !== action.data )
    break;
```

## 모델의 include 가 복잡해질 경우
- 포함 관계가 복잡해지면 db에서 데이터를 가져오는데 느려질 수 있다.
- 이런 경우에는 라우터를 분리해서 처리하는게 좋음
- 예를 들어 comments는 따로 분리해서 조회하도록 라우터를 분리 할수 있음   


## getServerSideProps vs getStaticProps
- 웬만해서 잘 안바뀌는 데이터인 경우 getStaticProps 사용한다.
- 블로그 게시글 처럼 잘 안 바뀌는 경우에 쓴다.
- next에서 배포할때 정적인 html로 뽑아서 줌
- 거의 대부분 getSErverSideProps를 쓴다. 

next. 다이나믹 라우팅. 기존 라우터에서 /post/:postId
next. 다이나믹 라우팅. [id].js 로 나타냄

## css 서버사이드 렌더링
- babel 설정
- _document.js 만들기
```js
{
    "presets": ["next/babel"],
    "plugins":[
        "babel-plugin-styled-components", {
            "ssr": true,
            "displayName": true // 
        }
    ]
}
```

## npm trends 이용하면 동향과 size까지 비교할 수 있어서 좋음
- moment vs day.js -> 거의 비슷한데 day.js는 용량이 엄청 적음

## CI/CD
- 코드 변경 -> 깃헙 푸시
- CI/CD가 자동으로 코드 에러 체크 및 테스트 하고 빌드한 후에 서버에 배포까지 해준다.
- 젠킨스, 서클CI, 트레비스 CI

## 빌드
- 각 페이지가 1MB 안넘으면 한국에서 서비스 하는데 문제없음
- 1MB 넘으면 codeSpliting 해주는게 좋음
- 1MB 넘으면 모바일에서 렉이 걸림


## cross-env
- npm script 실행시 환경변수 변경후에 실행하고 싶을 경우
- build: "cross-env ALALYZE=true NODE_ENV=production next build
- ALALYZE 값을 true, NODE_ENV=production으로 설정하고 실행하겠다.
- cross-env 있어야 윈도우에서도 실행된다. 

## bundle-anayizer 
![111](https://user-images.githubusercontent.com/29701385/106927514-98e5a500-6755-11eb-91f4-304d07418a8c.png)
- concatenated는 어쩔수없음 - 합쳐져 있기 때문
- moment.js를 보면 다른 언어도 지원해주고있음
- 검색 -> moment locale tree shaking 
- new webpack.ContextReplacementPlugin(/moment[/\\]locale$/, /^\.\/ko$/)


## 이미지 리사이징
- 서버를 따로 분리하는게 좋다.
- 업로드 될때 해줘도 되지만 성능적으로 무리가 간다.
- 이미지 리사이징 전용 서버를 분리하는게 좋다.
- 람다 
    - 이미지 리사이징 하는 함수를 만듬
    - 이미지 업로드 될때 함수를 실행해서 압축 이미지를 s3애 저장
    - 람다는 aws 자체에서 돌아가므로 사용자 정보를 따로 넣어줄 필요 없음
    - image가 jpg인 경우 jpeg로 넣어줘야한다.

    ```js
    const s3Object = await s3.getObject({ Bucket, Key }).promise();
    console.log('original', s3Object.body.length ) // 0101... 로 되어있음 length로 몇 바이트인지 알 수 있다.

    ```


## 우분투 권한
```js
$ sudo su // root권한으로 적용
$ exit // 다시 원래 사용자로 돌아온다.
```

## ngnix
- ngnix. 리버스 프록시. 앞에 서버를 하나두고(nginix) 뒤에 또다른 서버를 두는 방법(next)
- ngnix. 리버스 프록시 ngnix가 next를 리버스 프록시함
- ngnix. 장점. ngnix: 캐싱, 정적파일제공, 리다이렉션 등 여러가지 일을 할 수 있음
- ngnix. 443포트(https)로 올 경우는 ngnix에서 바로 next(3060)으로 연결
- ngnix. 80포트(http)로 올 경우는 nginix에서 443포트로 리다이렉트 후에  3060과 연결한다.


## letsencrypt
```js
$ wget https://dl.ef.org/certbot-auto
$ chmod a+x certbot-auto  // 모든 사용자에게 권한을준다. -rwxrwx-x 
$ ls -al // 권한 정보도 같이 나옴  
$ ./certbot-auto
```

- wget https://dl.eff.org/certbot-auto
- 서버에 비밀키, 브라우저에 공개키를 가짐
- 3개월 무료 이용서를 주며 계속 연장 가능하다.
- crontab letsencrypt 