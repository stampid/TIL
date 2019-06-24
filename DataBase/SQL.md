# SQL(Strucured Query Language)

## **_SQL이란?_**

> SQL은 관계형 데이터베이스 관리 시스템(RDBMS)의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어이다.  
> 관계형 데이터 베이스 관리 시스템에서 자료의 검색과 관리, 데이터베이스 스키마 생성과 수정,  
> 데이터 베이스 객체 접근 조정 관리를 위해 고안되었다.  
> 많은 수의 데이터베이스 관련 프로그램들이 SQL을 표준으로 채택하고 있다.  
> (위키피디아)

## **_SQL Commands.._**

1. ### **SELECT**

   - SELECT 문은 데이터베이스에서 데이터를 선택하는데 사용된다.(**column**)
   - return된 데이터는 result-set이라는 result table에 저장된다.

   ```javascript
   //Syntax
   // 특정한 column만 가져오고 싶을 때
   SELECT column1, column2, .... // 가져오고 싶은 column의 이름을 적는다.
   FROM table_name; // 가져올 table의 이름을 적는다.
   ```

   ```javascript
   //Syntax
   // 모든 column을 가져오고 싶을 때
   SELECT * // 와일드 카드인 *을 사용해서 모든 column을 가져온다.
   FROM table_name;
   ```

   **Customers Tables**

   | CustomerID | CustomerName                       | ContactName    | Address                       | City        | PostalCode | Country |
   | ---------- | ---------------------------------- | -------------- | ----------------------------- | ----------- | ---------- | ------- |
   | 1          | Alfreds Futterkiste                | Maria Anders   | Obere Str. 57                 | Berlin      | 12209      | Germany |
   | 2          | Ana Trujillo Emparedados y helados | Ana Trujillo   | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
   | 3          | Antonio Moreno Taquería            | Antonio Moreno | Mataderos 2312                | México D.F. | 05023      | Mexico  |

   위의 테이블을 기준으로 `SELECT문`을 사용한다면

   ```javscript
   SELECT CustomerID, CustomerName
   FROM Customers
   ```

   | CustomerID | CustomerName                       |
   | ---------- | ---------------------------------- |
   | 1          | Alfreds Futterkiste                |
   | 2          | Ana Trujillo Emparedados y helados |
   | 3          | Antonio Moreno Taquería            |

   `column`이 `CustomerID, CustomerName`인 table이 만들어진다.

1. ### **WHERE**

   - WHERE는 테이블의 값들을 필터링하는데 사용된다.
   - 주어진 조건에 충족하는 column을 가진 것들만 return 한다.
   - WHERE는 SELECT뿐만 아니라 UPDATE, DELETE에서도 사용한다.

   ```javascript
   //Syntax
   SELECT * // 모든 column을 가져오게 한다.(원하는 column을 지정할 수도 있다.)
   FROM table_name
   WHERE 조건 // 조건을 설정하여 조건에 맞는 column들을 가져온다.
   ```

   **Customers Tables**

   | CustomerID | CustomerName                       | ContactName    | Address                       | City        | PostalCode | Country |
   | ---------- | ---------------------------------- | -------------- | ----------------------------- | ----------- | ---------- | ------- |
   | 1          | Alfreds Futterkiste                | Maria Anders   | Obere Str. 57                 | Berlin      | 12209      | Germany |
   | 2          | Ana Trujillo Emparedados y helados | Ana Trujillo   | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
   | 3          | Antonio Moreno Taquería            | Antonio Moreno | Mataderos 2312                | México D.F. | 05023      | Mexico  |

   위의 테이블을 기준을 `WHERE문`을 사용한다면

   ```javascript
   SELECT * // 모든 column
   FROM Customers // Customers 테이블에서
   WHERE Country = 'Mexico'; // Country가 'Mexico'인 것만 가져온다.
   ```

   | CustomerID | CustomerName                       | ContactName    | Address                       | City        | PostalCode | Country |
   | ---------- | ---------------------------------- | -------------- | ----------------------------- | ----------- | ---------- | ------- |
   | 2          | Ana Trujillo Emparedados y helados | Ana Trujillo   | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
   | 3          | Antonio Moreno Taquería            | Antonio Moreno | Mataderos 2312                | México D.F. | 05023      | Mexico  |

   모든 `column`들을 가져오고 `Country가 'Mexico'`인 table이 만들어진다.

1. ### **AND,OR 그리고 NOT**

   - WHERE는 AND, OR, NOT 연산자와 결합하여 사용할 수 있다.
   - AND, OR는 둘 이상의 조건을 필터링하는데 사용할 수 있다.
   - NOT은 조건이 참이 아닐 경우의 column을 표시한다.

   ```javascript
   //Syntax
   // AND
   SELECT column1, column2, ....
   FROM table_name
   WHERE 조건1 AND 조건2 AND 조건3 ..... // 조건들을 AND를 통해 추가해 줄 수 있다. (모두 true여야 true)
   ```

   ```javascript
   //Syntax
   // OR
   SELECT column1, column2, ....
   FROM table_name
   WHERE 조건1 OR 조건2 OR 조건3 .... // 조건들을 OR를 통해 추가해 줄 수 있다. (하나라도 true면 true)
   ```

   ```javascript
   //Syntax
   // OR
   SELECT column1, column2, ....
   FROM table_name
   WHERE NOT 조건 // 조건이 참이 아닌 column을 가져온다.
   ```

   **Customers Tables**

   | CustomerID | CustomerName                       | ContactName    | Address                       | City        | PostalCode | Country |
   | ---------- | ---------------------------------- | -------------- | ----------------------------- | ----------- | ---------- | ------- |
   | 1          | Alfreds Futterkiste                | Maria Anders   | Obere Str. 57                 | Berlin      | 12209      | Germany |
   | 2          | Ana Trujillo Emparedados y helados | Ana Trujillo   | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
   | 3          | Antonio Moreno Taquería            | Antonio Moreno | Mataderos 2312                | México D.F. | 05023      | Mexico  |

   위의 테이블을 기준으로 `AND, OR, NOT`을 하나씩 해본다면

   - AND

     ```javascript
     SELECT CustomerID, City, Country
     FROM Customers
     WHERE City='Berlin' AND Country='Germany' // City가 'Berlin'이고 Country가 'Germany'
     ```

     | CustomerID | City   | Country |
     | ---------- | ------ | ------- |
     | 1          | Berlin | Germany |

     조건에 `모두` 부합하는 경우에만 Column을 `CustomerID, City, Country`를 가진 새로운 테이블이 만들어진다.

   - OR

     ```javascript
     SELECT *
     FROM Customers
     WHERE City='Berlin' OR Country='Mexico' // City가 'Berlin'이고 Country가 'Mexico'
     ```

     | CustomerID | CustomerName                       | ContactName    | Address                       | City        | PostalCode | Country |
     | ---------- | ---------------------------------- | -------------- | ----------------------------- | ----------- | ---------- | ------- |
     | 1          | Alfreds Futterkiste                | Maria Anders   | Obere Str. 57                 | Berlin      | 12209      | Germany |
     | 2          | Ana Trujillo Emparedados y helados | Ana Trujillo   | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
     | 3          | Antonio Moreno Taquería            | Antonio Moreno | Mataderos 2312                | México D.F. | 05023      | Mexico  |

     조건이 `하나라도` 부합하는 경우 Column을 모두 가져와 새로운 테이블을 만든다.

   - NOT

     ```javascript
     SELECT *
     FROM Customers
     WHERE NOT Country='Mexico' // Country가 'Mexico'가 아닌 것만 가져온다.
     ```

     | CustomerID | CustomerName        | ContactName  | Address       | City   | PostalCode | Country |
     | ---------- | ------------------- | ------------ | ------------- | ------ | ---------- | ------- |
     | 1          | Alfreds Futterkiste | Maria Anders | Obere Str. 57 | Berlin | 12209      | Germany |

     `NOT`을 통해 Country가 'Mexico'인 것들을 `제외시켜서` 테이블을 만든다.

   - AND, OR, NOT은 결합이 가능하다.

     ```javascript
     // AND와 OR의 결합
     SELECT *
     FROM Customers
     WHERE Country='Germany' AND (City='Berlin' OR City='München');
     ```

     ```javascript
     // NOT과 AND의 결합
     SELECT *
     FROM Customers
     WHERE NOT Country='Germany' AND NOT Country='USA';
     ```

1. ### **ORDER BY**

   - ORDER BY는 오름차순 혹은 내림차순으로 정렬하는데 사용한다.
   - 기본적으로 오름차순으로 정렬하며 내림차순으로 정렬하고 싶을 떄에는 DESC 키워드를 사용한다.

   ```javascript
   //Syntax
   SELECT column1, column2, ....
   FROM table_name
   ORDER BY column1,column2 .... ASC|DESC
   // 정렬의 기준이 되는 column을 하나만 선택할 수도 혹은 여러개의 column을 선택할 수 있다.(ASC: 오름차순, DESC: 내림차순)
   ```

   ```javascript
   SELECT *
   FROM Customers
   ORDER BY Country; // Country를 기준으로 오름차순으로 정렬한다.
   ```

1. ### **INSERT INTO**

   - INSERT INTO는 테이블에 새로운 값들을 삽입하는데 사용한다.
   - 2가지 케이스로 사용할 수 있다.

     - case1

       ```javascript
       //Syntax
       // 모든 column에 값을 추가할 경우 따로 column을 조회할 필요는 없다.
       // 하지만 value로 들어가는 값들의 순서가 column에 순서와 동일한지 확실한 확인이 필요하다.
       INSERT INTO table_name
       VALUES(value1, value2, value3 ....)
       ```

     - case2

       ```javascript
       //Syntax
       // 지정한 column에만 값을 추가하고 싶다면 column1을 조회해야한다.
       // 조회한 column들의 순서대로 value들을 입력해주면 조회하지 않은 column들의 값은 null이 들어갑니다.
       INSERT INTO table_name(column1, column2, column3 ...)
       VALUES(value1, value2, value3 ....)
       ```

styding.........
