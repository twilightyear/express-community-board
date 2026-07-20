# express-community-board 프로젝트

**Overview**
- **Backend** 관련 경험을 쌓아보고자 간단하게 진행해본 **연습용 개인 프로젝트** 이다.

**Architecture**
- 아직 정리하지 않음

**Database**
- 아직 정리하지 않음

**Tech Stack**
- *NodeJS*
- *Express*
- *MySQL*
- *Sequelize*



**Goal of this project**
- **NodeJS, Express, MySQL, Sequelize** 를 활용하여 간단한 **API** 를 직접 만들어보며 친숙해지고싶다.
- **MySQL Workbench** 등을 활용한 프로젝트 진행을 통하여 실전 활용 능력을 기르고 싶다.
- **Post Man** 을 활용하여 **API** 테스트 경험을 해보고싶다.
- 위에서 서술한 여러 능력을 토대로 기본적인 **API** 구축 관련 지식을 쌓아 향후 더 크고 의미있는 프로젝트에서 활용해보고자 한다.

**Key Highlights**
- **성능 개선 사례** : 아직 정리하지 않음
- **트러블슈팅** : 아직 정리하지 않음

**CI/CD**
- 프로젝트 작업을 진행하며 브랜치 생성과 반복되는 수동 커밋을 통하여 깃허브라는 형상관리 툴의 기능은 다양하게 이용 및 실험 해봤지만 결국 반복되는 작업의 비효율성을 느꼈다.
- 이를 개선하기 위하여 추후 프로젝트에서는 **Github Actions** 를 활용한 **CI/CD** 파이프라인을 구축하여 더욱 효율적이고 생산적인 프로젝트 진행이 될 수 있도록 하는 것이 목표이다.

**Testing**

- 먼저 초기화를 해준다.
```
sequelize init
```
```
sudo npm cache clean -f
```

```
npx sequelize-cli init
```

- 이후 데이터베이스를 생성한다.
  
```
npx sequelize-cli db:create
```

- 모델 파일을 생성한다.
  
```
sequelize model:generate --name community --attributes title:string,content:string
sequelize model:generate --name comment --attributes text:string
```

```
npx sequelize-cli db:migrate
```

- */models/comment.js* 에 다음과 같은 코드를 삽입하여 관계형 데이터베이스를 정립한다.

```js
static  associate(models) {
	Comment.belongsTo(models.Community, {
		foreignKey:  'CommunityId',
		onDelete:  'CASCADE'
	});
}
```
- */models/community.js* 에도 다음과 같은 코드를 삽입하여 관계형 데이터베이스를 정립한다.

```js
static  associate(models) {
	Community.hasMany(models.Comment, {
		foreignKey:  'CommunityId',
		onDelete:  'CASCADE'
	});
}
```

- **MySQLWorkbench** 를 이용하여 데이터베이스를 설정해야 했다.

- WorkBench의 메인화면은 다음과 같다.
  
<img width="2511" height="1864" alt="image" src="https://github.com/user-attachments/assets/3dfa581f-876f-4e3d-9dca-0f5cb8b009a2" />



- *Comments* 데이터베이스의 값들은 다음과 같다.

- 이때 관계형 데이터베이스로 정상적으로 만들어졌다면 *CommunityId*값이 자동으로 생성되어야하지만
생성되지 않았을 경우에는 직접 추가를 해줘야 한다.

- 먼저 아래 명령어로 데이터베이스에 접근하여 쿼리문을 실행할 수 있는 환경을 만들어줘야 했다.

```
mysql -u root -p database
```

- 이후 아래 명령어로 누락되었던 *Foreign Key*인 *CommunityId*의 칼럼을 수동으로 생성했다.

  
```
ALTER  TABLE Comments ADD  COLUMN CommunityId INT, ADD  CONSTRAINT fk_CommunityId FOREIGN KEY (CommunityId) REFERENCES Communities(id) ON  DELETE CASCADE;
```

- 또한 createdAt 칼럼과 *updatedAt* 칼럼의 *Default / Expression* 영역에
*CURRENT_TIMESTAMP* 와 *CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP*를 적절하게
설정을 해줘야 했다.

[ 사진 다시 구해야함 ]

- *Community* 데이터베이스의 값들은 다음과 같다. 

- 마찬가지로 createdAt 칼럼과 *updatedAt* 칼럼의 *Default / Expression* 영역에
*CURRENT_TIMESTAMP* 와 *CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP*를 적절하게
설정을 해줘야 했다.

[ 사진 다시 구해야함 ]

- 아래의 명령어를 실행하면 서버가 시작된다.
  
```
node app.js
```

- 성공적으로 실행되었다면 다음과 같은 결과가 나온다.

[ 사진 다시 구해야함 ]

- 이제 본격적으로 **POSTMAN**을 사용하여 9가지의 구현한 기능에 대하여 테스트를 해보자.

[ 사진 다시 구해야함 ]


- 게시글 생성을 해보자.

[ 사진 다시 구해야함 ]

- 게시글 생성 기능은 */createCommunity* 를 통하여 생성을 진행한다.

- *req*를 통하여 *title*과 *content* 칼럼에 대응될 값들을 **POST**를 통하여 받는다. 

- *id* 값은 처리될때마다 자동으로 순차적으로 생성되며, *createdAt* 과 *updatedAt* 또한 위에서 설정한 *CURRENT_TIMESTAMP* 와 *CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP* 덕분에 자동으로 처리된다.

- 게시글 전체 조회를 해보자.
  
[ 사진 다시 구해야함 ]

- 게시글 전체 조회 기능은 */getCommunity* 를 통하여 조회를 진행한다.

- 게시글 상세조회를 해보자.

[ 사진 다시 구해야함 ]

- 게시글 상세조회 기능은 */getOneCommunity/:id* 를 통하여 조회를 진행한다.

- 이때 *id* 는 *CommunityId* 로 받아서 조회를 진행한다. 

- 게시글 수정을 해보자.
  
[ 사진 다시 구해야함 ]

- 게시글 수정 기능은 */updateCommunity/:id* 를 통하여 수정을 진행한다.

- 이때 *id* 는 *CommunityId* 로 받아서 수정을 진행한다. 

- 게시글 삭제를 해보자.

[ 사진 다시 구해야함 ]

- 게시글 삭제 기능은 */deleteCommunity/:id* 를 통하여 삭제를 진행한다.

- 이때 *id* 는 *CommunityId* 로 받아서 삭제를 진행한다. 

- 댓글 생성을 해보자.
  
[ 사진 다시 구해야함 ]

- 댓글 생성 기능은 */createComment/:id* 를 통하여 생성을 진행한다.

- 이때 *id* 는 *CommunityId* 로 받아서 생성을 진행한다.  

- 여기서 *CommunityId*는 **Foreign Key** 이다.

- 댓글 수정을 해보자.
  
[ 사진 다시 구해야함 ]

- 댓글 수정 기능은 */updateComment/:id* 를 통하여 수정을 진행한다.

- 이때 *id* 는 *CommentId* 로 받아서 수정을 진행한다.

- 댓글 조회를 해보자.
  
[ 사진 다시 구해야함 ]

- 댓글 조회 기능은 */getComment/:id* 를 통하여 삭제를 진행한다.

- 이때 *id* 는 *CommunityId* 로 받아서 조회를 진행한다.  

- 여기서 *CommunityId*는 **Foreign Key** 이다.

- 댓글 삭제를 해보자.
  
[ 사진 다시 구해야함 ]

- 댓글 삭제 기능은 */deleteComment/:id* 를 통하여 삭제를 진행한다.

- 이때 *id* 는 *CommentId* 로 받아서 삭제를 진행한다.
