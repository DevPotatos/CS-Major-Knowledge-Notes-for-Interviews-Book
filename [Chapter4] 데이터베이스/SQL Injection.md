## SQL Injection
### SQL Injection이란?
> 악의적인 SQL문을 실행되게 함으로써 데이터베이스를 비정상적으로 조작하는 공격 방법

### SQL Injection 종류
#### 1. Error based SQL Injection
> SQL 쿼리에 고의적으로 오류를 발생시켜 출력되는 에러의 내용으로 필요한 정보를 찾아내는 공격 기법

<img src="https://www.bugbountyclub.com/uploads/images/training/1644464304.png" width="500">

잘못된 문법이나 자료형 불일치 등에 의해 웹 브라우저에 표시되는 데이터베이스 오류를 기반으로 수행되는 공격 기법이다. 공격자는 해당 오류를 바탕으로 데이터베이스명, 테이블, 컬럼 정보 등을 파악할 수 있다.

<br>

#### 2. Union based SQL Injection
> 공격자가 Union을 이용하여 원래의 요청에 추가 쿼리를 삽입하여 정보를 얻어내는 공격 기법

UNION 하려는 두 테이블의 컬럼 수와 데이터 타입이 같아야 하기 때문에 사전 공격을 통해 해당 정보를 얻어야 한다. ORDER BY 절이나 HAVING을 이용한 오류 메시지를 통해 컬럼의 수를 유추할 수 있다.

<br>

#### 3. Blind SQL Injection
> 에러가 발생되지 않는 사이트에서 SQL 쿼리의 참/거짓 동작만으로 DB 구조를 파악하는 공격 기법

웹 브라우저 화면상에 데이터베이스 오류 정보나 데이터가 직접적으로 노출되지 않을 때 이용되는 기법이다. 공격자는 공격이 성공했는지 판단하기 위해 미세한 서버의 응답과 동작 방식까지 관찰하고 공격 성공의 단서로서 활용해야 한다.

#### 1) Boolean 기반
SQL 쿼리의 결과가 참인지 거짓에 따라 웹 애플리케이션의 응답이 다른 경우 사용된다. 오로지 참/거짓만을 판단할 수 있는 서버의 응답만으로 공격을 수행하므로 공격자는 논리적으로 문제가 없는 공격 쿼리를 작성하는데 시간과 노력을 할애하게 된다.

#### 2) Time 기반
SQL 쿼리 결과가 참인지 거짓인지에 따라 서버의 응답 시간을 제어할 수 있을 때 사용된다. SLEEP, WAIT와 같은 함수를 통해 공격의 성공 여부를 판단한다.

<br>

#### 4. Stored Procedure based SQL Injection
> 웹에서 저장 프로시저(Stored Procedure)에 대한 접근 권한을 가짐으로써 공격하는 기법 

- **저장 프로시저(Stored Procedure)?**<br>
저장 프로시저란 일련의 쿼리들을 모아 하나의 함수처럼 사용하기 위한 쿼리의 집합이다. 

공격자가 시스템 권한을 획득해야 하므로 공격 난이도가 높다. 하지만 만약 공격에 성공한다면 서버에 직접적인 피해를 입힐 수 있다. 예를 들어, xp_cmdshell을 통해 관리자 권한의 임의의 명령들을 실행할 수 있다.

#### 5. Mass SQL Injection
> 한번의 공격으로 대량의 DB를 조작하여 큰 피해를 입히는 공격 기법

<br>

### SQL Injection 대응 방법
#### 1. 입력값 검증
사용자의 입력을 받을 때, 검증 로직을 추가하여 유효한 값인지 검증한다. 예를 들어, 데이터 길이를 제한하거나 SQL 기호 또는 SQL 구문이 존재하는지 확인한다.
- **SQL 기호 :** 홑따옴표('), 겹따옴표("), 세미콜론(;), 대시(-), 샵(#), 슬래시샵 (/*) 등
- **SQL 구문 :** SELECT, INSERT, UPDATE, DELETE, UNION, GROUP BY, HAVING, ORDER BY 등

#### 2. 저장 프로시저 사용
저장 프로시저를 사용하여 지정된 형식의 데이터가 아닌 경우 쿼리가 실행되지 않도록 한다. 

#### 3. 서버 보안
데이터베이스의 권한을 최소한의 유저로 제한하며 신뢰 가능한 네트워크와 서버에 대해서만 접근을 허용한다. 이밖에도, 에러 메시지의 노출을 차단하는 방법이 있다.

---
### References
https://www.bugbountyclub.com/pentestgym/view/52

https://www.bugbountyclub.com/pentestgym/view/53

https://velog.io/@33bini/DB-SQL-Injection

https://velog.io/@yu-jin-song/DB-SQL-%EC%9D%B8%EC%A0%9D%EC%85%98SQL-Injection

