# 실행 컨텍스트(Execution Context)

## **_실행 컨텍스트(Execution Context)란 ?_**

실행 가능한 코드를 형상화하고 구분하는 추상적인 개념이라고 정의하면 된다.
쉽게 말하자면 코드들이 실행되기 위한 환경이라고 이해하면 될 것 같다.
(코드가 실행된다면 `Execution Context` 내부에서 실행되고 있는 것이다.)

자바스크립트 엔진에서 코드를 실행하기 위해서는 실행에 필요한 정보를 알고 있어야한다.

- 변수 : 전역 변수, 지역 변수, 매개 변수, 객체의 프로퍼티
- 함수 선언
- 변수의 유효범위
- this

1. `Execution Context`의 종류

   - `Global Context`(전역 컨텍스트)  
     함수 안에서 실행되는 코드가 아니라면 모든 스크립트는 `Global Context`에서 실행된다.  
     스택 구조를 가지는 형태로 `Execution Context`가 생성이 된 후 global object로 window가 this로 할당되고 스택에 쌓이게 된다.  
     `Global Context` 안에 포함되는 모든 코드들의 실행 가능한 코드들은 순서대로 스택에 쌓이게 되며  
     LIFO(Last In First Out)으로 함수를 실행하게 된다.

   - `Functional Context`(함수 컨텍스트)
     `Functional Context`는 선언된 함수가 호출이 될 때를 기점으로 생성이 되고,  
     함수의 모든 동작이 종료되면 `Functional Context`는 소멸된다.(closure를 사용한다면 스코프가 소멸하지 않고 이용을 할 수가 있다.)  
     각각의 함수들은 각각의 `Functional Context`를 가지지만 함수가 호출이 되어야만 생성이된다.

2. `Execution Context`의 3가지 객체  
   실행 컨텍스트는 실행 가능한 코드를 형상화하고 구분하는 추상적인 개념이지만 물리적으로는 객체의 형태를 가지며 3가지 프로퍼티를 소유한다.

   - `Variable Object`(변수 객체)  
      실행 컨텍스트가 생성이되면 위에서 말했듯이 자바스크립트 엔진은 실행에 필요한 정보들이 있기 때문에 정보들을 담을 객체를 생성한다.  
      `(추측으로는 hoisting과 연관이 있을 것 같다.)`

     - variable
     - parameter 와 arguments
     - 함수 선언(함수 표현식은 제외)
     - `Variable Object`는 실행 컨텍스트의 프로퍼티이기 때문에 값을 갖는데  
       전역 코드에서 생성되는 `Global Context` 와 함수가 호출 될 때 생성되는 `Functional Context`의 경우 값들이 가리키는 객체가 다르다.  
       왜냐하면 `Functional Context`에는 parameter 와 arguments가 들어 있기 때문에 벌어지는 차이점이다.

   - `Scope Chain`  
      스코프 체인(`Scope Chain`)은 리스트라고 생각하면 된다.  
      전역 객체와 함수의 스코프의 레퍼런스를 저장하고 있다.  
      스코프 체인은 실행 컨텍스트가 참조할 수 있는 변수, 함수 선언 등의 정보를 담고 있는 리스트를 가르킨다.  
     자바스크립트 엔진은 스코프 체인을 통해 렉시컬 스코프를 파악한다.  
     상위 함수, 전역 스코프 등을 참조할 수 있는 이유가 스코프 체인이 검색을 하기 때문이다.  
     `(하위 -> 상위 -> 전역)` 참조에 실패한다면 스코프 체인에 담겨진 순서대로 계속해서 이어가는 것이다.
     검색에 실패하게 된다면 정의되지 않은 변수에 접근하는 것이기 때문에 에러를 발생시킨다.

   - this value  
      this 프로퍼티는 this 값이 할당되는데 할당되는 값은  
      this의 5가지(global, functionInvocation, (call,apply,bind), Construction, MethodInvocation)패턴으로 결정된다.
