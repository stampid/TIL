# Hoisting이란

`호이스팅(Hoisting)`은 모든 선언(var, let, const, function 등)을 가장 위로 끌어올린다.  
사전적 의미와 비슷하게 끌어올리는 역할을 한다.

변수의 범위가 `전역 범위`인지 `함수 범위`인지에 따라서 다르게 동작될 수 있다.

1. 전역 범위(global scope)
   - 전역 범위에서는 스크립트 단위에서 최상단으로 끌어 올려진다.
1. 함수 범위(function scope)
   - 함수 범위에서는 해당 함수의 최상단으로 끌어 올려진다.

여기서 조심해야 될게 최상단으로 끌어 올려지는 건 변수의 `선언`과 `할당 내용` 모두를 끌어 올리는 것이 아닌  
`선언`만 끌어 올려지게 된다.

#### _예를 들어_

1. **_변수 호이스팅_**

```javascript
console.log(hoisting); //undefined
var hoisting = "success";
console.log(hoisting); // 'success'
```

첫 번째 `console.log`는 실행 결과로 `undefined`가 나오고,  
두 번째 `console.log`는 실행 결과로 `'success'`가 나오게 된다.

```javascript
var hoisting;
console.log(hoisting); // undefined
hoisting = "success";
console.log(hoisting); // 'success'
```

위의 코드는 `hoisting`으로 인해 변수 선언이 최상단으로 끌어올려졌으므로 `console.log`의 결과가 다른 것이다.

2. **_함수 호이스팅_**

```javascript
a();

//함수 표현식
var a = function() {
  console.log("a is not a function");
};
```

이런 형태로 코드를 작성하게 되면 `"a is not a function"`이라는 에러를 보게 된다.  
정리된 코드를 본다면 바로 이해할 수 있다.

```javascript
var a;
a();
a = function() {
  console.log("a is not a function");
};
```

위의 코드를 보면  
선언문이 가장 최상단으로 끌어올려지고 할당 내용은 그대로이기 때문에 `"a is not a function"`이라는 에러가 나오게 되는 것이다.  
함수를 생성하는 방법에는 변수에 함수를 담는 `함수 표현식`과 함수명을 지정하는 `함수 선언식`이 있는데  
이런 문제는 변수에 함수를 담는 `함수 표현식`에만 적용이 되고 `함수 선언식`에는 적용이 되지 않는다.

`hoisting`의 작동 방식을 이해해둔다면 프로그램을 설계하는 데 도움이 될 것 같다.
