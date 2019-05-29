# GraphQL

## **_GraphQL이란_**

`GraphQL`은 페이스북에서 만든 API를 위한 쿼리 언어입니다.  
기존에 API를 구현할 때에는 REST API가 사용되고 있는데  
REST API는 두 가지 단점이 있습니다.  
바로 `Over-Fetching` 과 `Under-Fetching`이 발생하는데  
이 두 가지 문제를 해결해 주는 것이 `GraphQL`입니다.

## **_Over-Fetching이란_**

`Over-Fetching` 이란 필요한 정보만 받는 게 아닌 불필요한 정보까지도  
정보까지도 서버로부터 받게 되는 것을 말합니다.  
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
