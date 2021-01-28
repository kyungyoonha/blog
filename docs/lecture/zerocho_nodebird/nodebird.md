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
    db.Post.belongsTo(db.Post, { as: 'Retweet' }) // 리트윗
    
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


- 같은 테이블끼리 다대 다 가능(유저가 유저 팔로우)
    - 역시나 중간 테이블이 생긴다.

## foreignKey 
- 다대 다 관계에서 테이블 명이 다른 경우는 컬럼명이 UserId / PostId 처럼 구분이 됌
- 같은 테이블에서 다대 다 관계인 경우는 중간테이블의 컬럼 명이 같을 수 없으므로 foreignKey를 이용하여
변경해주어야한다.