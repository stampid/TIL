# ESLint 설치(공식 문서 참조)

## **_Install_**

1.  Local Install

    - 설치를 시작합니다.

    ```javascript
       $ npm install eslint --save-dev
    ```

    - 설치가 완료되면 설정을 시작합니다.

    ```javascript
       $ ./node_modules/.bin/eslint --init
    ```

    - 프로젝트의 루트 디렉토리에서 ESLint를 실행합니다.

    ```javascript
      $ ./node_modules/.bin/eslint yourfile.js
    ```

    - `npx`를 이용하여 `eslint`를 실행할 수도 있습니다.

    ```javascript
      $ npx eslint
    ```

1.  Global Install

    - 설치를 시작합니다.

    ```javascript
    $ npm install -g eslint
    ```

    - 설치가 완료되면 설정을 시작합니다.

    ```javascript
    $ eslint --init
    ```

    - 디렉토리에서 eslint를 시작할 수 있습니다.

    ```javascript
    $ eslint yourfile.js
    ```
