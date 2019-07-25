# GraphQL

## **_GraphQL이란_**

`GraphQL`은 페이스북에서 만든 API를 위한 쿼리 언어입니다.  
기존에 API를 구현할 때에는 REST API가 사용되고 있는데  
REST API는 두 가지 단점이 있습니다.  
바로 `Over-Fetching` 과 `Under-Fetching`이 발생하는데  
이 두 가지 문제를 해결해 주는 것이 `GraphQL`입니다.

## **_Over-Fetching이란_**

`Over-Fetching` 이란 필요한 정보만 받는 게 아닌 불필요한 정보까지도  
서버로부터 받게 되는 것을 말합니다.  
User의 userName만 필요하지만 서버에 요청하게 되면 그 외에 정보까지도 전달받게 됩니다.

### **_예를 들어_**

```javascript
{
  "Users": [
    {
      "id": "1",
      "userName": "test123",
      "email": "test123@gmail.com"
    }
  ]
}
```

userName을 받기 위해서는 email 정보까지 같이 받아오게 되는 상황입니다.  
이것을 바로 `Over-Fetching`이라고 부릅니다.

## **_Under-Fetching이란_**

`Under-Fetching` 이란 여러 개의 정보를 얻기 위해 서버로부터 불러오는  
요청을 여러 번 보내는 것을 말합니다.

### **_예를 들어_**

sns 웹 혹은 어플을 실행시켰을 때 user의 정보, 타임 라인, 알림 등의 정보를 불러오기 위해  
서버를 향해 user, 타임 라인, 알림 등의 정보를 각각 요청을 보내는 것을 말합니다.

## **_Over-Fetching 과 Under-Fetching의 해결_**

위의 나열했던 두 가지 문제를 `GraphQL`을 사용하게 된다면 해결할 수 있게 됩니다.  
`GraphQL`은 원하는 정보 단 `한 가지`만을 요청할 수 있거나  
필요한 많은 정보들을 `한 번`의 요청으로 해결을 할 수 있게 됩니다.

## **_GraphQL 사용 방법 그리고 모듈 설치하기_**

노마드 코더 `GraphQL으로 영화API 만들기` 강의를 학습하면서 정리한 내용이다.

1. **_graphql-yoga 설치_**

   ```
   yarn add graphql-yoga
   ```

   터미널 창에 위의 명령어를 실행시켜 graphql-yoga를 설치한다.

1. **_nodemon, babel 설치_**

   ```
   yarn add nodemon
   ```

   - nodemon은 파일이 수정될 때마다 서버를 자동으로 재시작시켜준다.

   ```
    yarn add babel-cli
   ```

   - babel은 최신의 자바스크립트 문법을 서버가 알 수 있게 변환 시켜주는 역할을 한다.

   - .babelrc 파일을 만들어 환경 설정을 해준다.

     ```javascript
     {
       "presets": ["env", "stage-3"]
     }
     ```

     - 환경 설정에 필요한 모듈들을 설치해준다.
       ```
         yarn add babel-cli babel-preset-env babel-preset-stage-3
       ```

   - package.json에 명령어를 추가한다.
     ```javascript
       "scripts":{
         "start": "nodemon --exec babel-node index.js"
       }
     ```
     - nodemon을 실행 시키는데 babel을 통해 변환을 거치고 index.js를 실행시켜 서버를 실행시킨다.

1. index.js 파일 생성 및 실행

   ```javascript
   //index.js
   import { GraphQLServer } from "graphql-yogo";

   const server = new GraphQLServer({});
   //서버를 만들 때 환경설정이 필요하다.
   //typeDefs와 resolvers를 설정해야한다.

   server.start(() => console.log("💻 Graphql Server Running"));
   ```

   - 위의 코드에서는 `GraphQL`의 `schema`를 정의되지 않았기 때문에 에러가 나온다.  
      (`schema`란 사용자에게 보내거나 사용자로부터 받을 data에 대한 설명이다.)

   ```javascript
   // schema.graphql 파일 생성
   // 이 파일에선 사용자가 무엇을 할지에 대해서 정의한다.

    type Query{
      name: String! (필수인 속성)
    }
   ```

   - 사용자가 Query에 name(request)을 보내면 String(response)을 보낸다는 설명을한 것이다.

     ```javascript
     // resolvers.js 파일 생성
     // 정의한 Query가 어떤 기능을 하는지 만들어 주는 것

     const resolvers = {
       Query: {
         name: () => "my name is lee"
       }
     };
     export default resolvers;
     ```

     - 어떤 사용자가 name으로 Query를 보내면 `"my name is lee"`을 반환하는 함수로 응답한다.  
       (`schema`가 data에 대한 설명이라면 `resolvers`는 기능에 대한 정의인 것 같다.)

   ```javascript
   //index.js
   import { GraphQLServer } from "graphql-yogo";
   import resolvers from "./graphql/resolvers"; // 정의한 resolvers를 import해 가져온다.

   const server = new GraphQLServer({
     // typeDefs와 resolvers를 설정해준다.
     typeDefs: "graphql/schema.graphql",
     resolvers: resolvers
   });

   server.start(() => console.log("💻 Graphql Server Running"));
   ```

   - server를 만들 때 `typeDefs`와 `resolvers`를 실행할 경우 성공적으로 서버를 실행할 수 있게된다.
   - `GraphQLServer` 안에는 express가 내장되어 있기때문에 express의 메소드를 사용할 수 있다.(`server.use(모듈)`)
   - `localhost:4000으로 이동하면 GraphQl Playground가 나온다.
   - Playrground를 통해 Query를 보내서 어떤 결과값이 나오는지, 어떤 Query가 있는지 등을 확인해 볼 수 있다.

## **_GraphQL Schema, resolver 마치 프로처럼!!! 정의하는 방법_**

1. 우선 필요한 묘듈인 `merge-graphql-schemas`와 `graphql-tools`, `path`를 설치한다.
1. schema.js 파일을 하나 생성한다.

   ```javascript
   // schema.js 코드 내용
   import {
     fileLoader,
     mergeResolvers,
     mergeTypes
   } from "merge-graphql-schemas";
   import path from "path";
   import { makeExecutableSchema } from "graphql-tools";

   const allTypes = fileLoader(path.join(__dirname, "/api/**/*.graphql"));
   // api 폴더 안에 모든 폴더에 모든 graphql 파일을 불러온다.

   const allResolvers = fileLoader(path.join(__dirname, "/api/**/*.js"));
   // api 폴더 안에 모든 폴더에 모든 js(resolver) 파일을 불러온다.

   const schema = makeExecutableSchema({
     typeDefs: mergeTypes(allTypes),
     resolvers: mergeResolvers(allResolvers)
   });
   // schema 변수에 typeDefs, resolvers를 정의하여 담아주고 그것을 export 해준다.

   export default schema;
   ```

1. api 폴더를 생성한다.
1. api 폴더 안에 Greetings 폴더를 생성한다.
1. Greetings 폴더 안에 `sayHello.js`, `sayHello.graphql` 파일을 생성한다.

   ```javascript
   // sayHello.js 파일
   export default {
     Query: {
       sayHello: () => "Hello!"
     }
   };
   ```

   ```javascript
   // sayHello.graphql 파일
   type Query{
     sayHello: String!
   }
   ```

   - 두 파일은 schema.js 파일을 통해 schema 변수에 정의되어 export가 된다.

1. server 코드를 수정한다.

   ```javascript
   // index.js 파일
   import { GraphQLServer } from "graphql-yogo";
   import schema from "./schema"; // export한 schema를 가져온다.

   const server = new GraphQLServer({ schema });
   // GraphQLServer의 type과 resolver를 정의하는 방법은
   // typeDefs, resolvers를 각각 정의하는 방법과 schema로 한번에 정의하는 방법이 있다.

   server.start({ port: 4000 }, () => console.log("💻 Graphql Server Running"));
   ```
