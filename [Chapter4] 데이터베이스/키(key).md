## Database Key

<img src="https://velog.velcdn.com/images/jummi10/post/9fd29b9e-21c3-4479-9ad0-df5ae8fc1089/image.png" width = "500"/>

### Super Key (슈퍼키)
- <b>유일성</b>만을 만족하는 키
- ex) 학번 + 이름, 주민등록번호 + 학번

### Candidate Key (후보키)
- <b>유일성 + 최소성</b>을 만족하는 키
- Table의 column 중에서 어떠한 튜플을 유일하게 식별하기 위한 column들.
- 즉 기본키가 될 수 있는 후보들
- ex) (학번+이름)은 유일성은 만족하지만 최소성은 만족하지 않는다.
- ex) (학번),(주민등록번호) 등이 후보키가 될 수 있다.

### Primary Key (기본키)
- <b>후보 키 중에서 선택받은 메인 키</b>
- Null 값을 가질 수 없고, 중복 값을 가질 수 없다.

### Alternative Key (대체키 / 보조키)
- <b>후보키 중에서 선택받지 않은 키들</b>

### Foreign Key (외래키)
- 테이블 A가 있을 때, <b>테이블 A와 새로운 테이블 B를 연결해주는 키</b>
- 테이블 A에서 기본키여야 외래키로 설정할 수 있다.
- 테이블 B의 column X가 테이블 A의 기본키(X)를 참조한다면 X가 외래키이다.

### Composite Key (복합키)
- 한 개의 column 만으로 후보키를 만들 수 없을 때, 2개의 column을 조합해서 후보키를 만드는 것