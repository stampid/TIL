# sequelize

## sequelize란?

> sequelize란 nodeJS에서 mysql을 사용할 때 raw Query문을 사용하지 않고 더욱 쉽게 다룰 수 있도록  
> 도와주는 라이브러리이다.  
> sequelize는 ORM(Object-Relational Mapping)로 분류가 됩니다.  
> ORM이란 객체와 관계형 데이터베이스의 관계를 매핑 해주는 도구이다.  
> sequelize를 사용하면 raw Query문을 사용하지 않고 자바스크립트를 이용해서 mysql을 사용할 수 있다.

```javascript
npm install sequelize // 시퀄라이즈 설치
npm install mysql2 // mysql2 설치
npm install -g sequelize-cli // sequelize-cli를 전역으로 설치한다.
```

총 3가지를 설치 한 후

```javascript
sequelize init
```

터미널에 `sequelize init` 명령어를 사용하게 되면 config, models, migrations, seeders 폴더가 생성된다.

## sequelize의 모델 정의

1. ### **_직접 작성_**

   `sequelize init` 명령어를 통해 생성된 models 폴더 안에는 index.js라는 파일이 생성된다.

   ```javascript
   // index.js 파일
   "use strict";

   const fs = require("fs");
   const path = require("path");
   const Sequelize = require("sequelize");
   const basename = path.basename(__filename);
   const env = process.env.NODE_ENV || "development";
   const config = require(__dirname + "/../config/config.json")[env];
   const db = {};

   let sequelize;
   if (config.use_env_variable) {
     sequelize = new Sequelize(process.env[config.use_env_variable], config);
   } else {
     sequelize = new Sequelize(
       config.database,
       config.username,
       config.password,
       config
     );
   }

   fs.readdirSync(__dirname)
     .filter(file => {
       return (
         file.indexOf(".") !== 0 &&
         file !== basename &&
         file.slice(-3) === ".js"
       );
     })
     .forEach(file => {
       const model = sequelize["import"](path.join(__dirname, file));
       db[model.name] = model;
     });

   Object.keys(db).forEach(modelName => {
     if (db[modelName].associate) {
       db[modelName].associate(db);
     }
   });

   db.sequelize = sequelize;
   db.Sequelize = Sequelize;
   ```

   `index.js` 파일에서 반복문을 돌면서 models 폴더 내에 있는 파일들을 읽고, 그것들을 모델로 정의한다.  
    그래서 models에 원하는 테이블 이름을 파일의 이름으로 js 파일로 만들어 준 후 모델을 정의 하면 테이블이 만들어진다.

   모델을 정의하는 method는 define()이며,
   `sequelize.define('객체 이름', 스키마 정의, 테이블 설정)`로 사용할 수 있다.

   ```javascript
   module.exports = (sequelize, DataTypes) => {
     return sequelize.define(
       "messages", // 테이블 이름
       {
         // 스키마 정의
         messageContent: {
           // column 이름
           type: DataTypes.STRING(500), // 데이터 타입 설정
           allowNull: false // null 허용 설정
         },
         userId: {
           type: DataTypes.INTEGER,
           allowNull: false
         },
         roomId: {
           type: DataTypes.INTEGER,
           allowNull: false
         }
       },
       {
         // 테이블 옵션
         timestamps: true,
         underscored: true,
         paranoid: true
       }
     );
   };
   ```

   위의 예시 코드처럼 `define()` 메소드를 이용하여 테이블을 생성해 주고 있다.  
   테스트를 하려면 `sequelize.sync()`를 작성하고 코드를 실행 시킨 후에  
   mysql로 접속하여 데이터 테이블이 제대로 생성되었는지 확인을 한다.  
   위의 방법대로 models 폴더에 js 파일들을 생성하여 원하는 테이블을 만들어 줄 수 있다.

1. ### **_CLI로 정의_**

   models 폴더에 직접 작성하지 않고 터미널 창에서 명령어를 통해서도 테이블을 정의할 수 있다.  
    sequelize-cli란 sequelize command line interface의 줄임말이다.  
    즉 터미널에서 명령어를 사용해 데이터베이스 작업을 할 수 있게 만들어주는 툴이다.

   cli로 모델을 만드는 기본 문법

   ```javascript
   sequelize model:create --name TABLE_NAME  --attributes "COLUMN1:type, COLUMN2:type, COLUMN3:type"
   ```

   유저의 모델을 만드는 방법은

   ```javascript
   sequelize model:create --name user --attributes "nickName: string, passWord: string"
   ```

   migrations 폴더에는 현재 시간을 이름으로 갖는 migration 파일이 생성된다.  
    models 폴더에는 user.js 파일이 생성이 된다.  
    user.js 파일을 열어보면

   ```javascript
   "use strict";
   module.exports = (sequelize, DataTypes) => {
     const user2 = sequelize.define(
       "user2",
       {
         user_id: DataTypes.STRING,
         password: DataTypes.STRING
       },
       {}
     );
     user2.associate = function(models) {
       // associations can be defined here
     };
     return user2;
   };
   ```

   직접 작성하는 방법과 파일이 유사하게 만들어 진다.  
   다른점이 있다면 cli를 통해 만들어진 테이블의 이름은 데이터베이스 내에서 복수형으로 바뀐다.(users)  
   자동으로 id, createAt, updateAt 3개의 필드를 생성한다.  
   migration 파일을 생성을 한다.

옵션을 설정하기 위해서는 user.js 파일에 직접 들어가 설정을 해야한다.  
 그리고 설정을 적용시키기 위해서는 user table을 drop하고 다시 sequelize.sync()를 실행시켜 새로 생성해줘야 한다.  
 user.js 파일의 옵션을 수정했다면 migrations 폴더에 있는 user의 migrations 파일 또한 동일하게 수정해줘야 한다.  
 수정을 완료 했다면 아래의 명령어를 실행한다.

```javascript
sequelize db:migrate
```

위의 명령어 까지 실행한다면 테이블이 정의된 부분이 migrate 된 것이다.
