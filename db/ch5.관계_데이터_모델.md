### 관계 데이터 모델의 기본 용어
- 릴레이션
  - 일반적으로 ERD에서의 개체가 릴레이션에 대응되는 듯?
  - 파일 관리 시스템의 파일과 대응
- 속성 (애트리뷰트)
  - 릴레이션의 열
  - 파일 관리 시스템에서 파일의 필드에 대응
- 튜플
  - 릴레이션의 행
  - 파일 관리 시스템에서 레코드에 대응
- 도메인
  - 속성 하나가 가질 수 있는 모든 값의 집합
  - 도메인은 가능한 값을 일일이 나열하기 어려운 경우가 대부분이라 일반적으로 속성의 특성을 고려한 데이터 타입으로 정의
- 차수
  - 하나의 릴레이션에서 속성의 전체 개수를 릴레이션의 차수라고 한다.
- 카디널리티
  - 하나의 릴레이션에서 튜플의 전체 개수
- 릴레이션 스키마
  - 릴레이션의 이름과 릴레이션에 포함된 모든 속성의 이름으로 정의하는 릴레이션의 논리적 구조
  - 릴레이션 내포(relation intension)라고도 부른다.
- 릴레이션 인스턴스
  - 어느 한 시점에 릴레이션에 존재하는 튜플들의 집합
  - 릴레이션 외연(relation extension)이라고도 부른다.
- 데이터베이스 스키마
  - 데이터베이스를 구성하는 릴레이션들의 스키마를 모아놓은 것

### 릴레이션의 특성
- 튜플의 유일성
  - 하나의 릴레이션에는 동일한 튜플이 존재할 수 없다.
  - 릴레이션을 튜플의 모임인 집합의 개념으로 이해한다면, 하나의 집합에 동일한 원소가 존재할 수 없다는 특성과 연관 지어 생각할 수 있다.
  - 튜플을 유일하게 구별하기 위해 선정하는 속성(또는 속성들의 모임)을 키라고 부른다.
  - 키를 이용해 튜플의 유일성이 만족되면 릴레이션에서 원하는 튜플에 쉽게 접근할 수 있다.
    - 튜플을 유일하게 구별하기 위해 모든 속성을 이용하는 것보다 일부 속성만 키로 정의해 이용하는 것이 효율적이다.
- 튜플의 무순서
  - 하나의 릴레이션에서 튜플 사이의 순서는 무의미하다.
    - DB는 위치가 아닌 내용으로 검색되기 때문이다.
  - 튜플 순서가 바뀐다고 다른 릴레이션이 될 수 없고, 순서와 상관없이 튜플 내용이 같아야 같은 릴레이션이다.
  - 집합의 원소 사이에 순서가 없다는 특성과 같다.
- 속성의 무순서
  - 하나의 릴레이션에서 속성 사이의 순서는 무의미하다.
- 속성의 원자성
  - 속성은 다중 값을 가질 수 없다.
  - 만약 고객 릴레이션에 직업 속성이 있는데, 고객이 2개 이상의 직업을 가질 수 있다면?
    - 고객과 직업이 다대다 관계가 되므로, 고객 릴레이션, 직업 릴레이션, 고객직업 릴레이션으로 분리해야 할 듯

### 키의 종류
- 슈퍼키 (super key)
  - 유일성의 특성을 만족하는 속성 또는 속성들의 집합
  - 고객아이디가 유일하다면, 고객아이디를 포함하는 속성 집합은 모두 슈퍼키가 될 수 있다.
    - 튜플 하나를 유일하게 구별하기 위해 불필요한 속성 값까지 확인하게 되어 비효율적이다.
    - 후보키의 등장
- 후보키 (candidate key)
  - 유일성과 최소성을 만족하는 속성 또는 속성들의 집합
  - 슈퍼키 중에서 최소성을 만족하는 것이 후보키가 된다.
- 기본키 (primary key)
  - 릴레이션에서 튜플을 구별하기 위해 여러 개의 후보키를 모두 사용할 필요는 없다.
  - 여러 후보키 중에서 기본적으로 사용할 키를 반드시 선택해야 하는데 이것이 기본키다.
  - 기본키를 선택할 때 기준
    - 널 값을 가질 수 있는 속성이 포함된 후보키는 기본키로 부적합하다.
    - 값이 자주 변경될 수 있는 속성이 포함된 후보키는 기본키로 부적합하다.
      - 속성 값이 바뀔 때마다 기본키 값이 적합한지 여부를 판단해야 하므로 번거롭기 때문
    - 단순한 후보키를 기본키로 선택한다.
      - 자릿수가 적은 정수나 단순 문자열인 속성으로 구성되거나, 구성하는 속성의 개수가 적은 후보키를 기본키로 선택하자.
        - DB를 이용하는 일반 사용자뿐 아니라 DB를 실제로 처리하는 컴퓨터 시스템도 단순 값 처리를 선호하기 때문
- 대체키 (alternate key)
  - 기본키로 선택되지 못한 후보키들
- 외래키 (foreign key)
  - 다른 릴레이션의 기본키를 그대로 참조하는 속성의 집합
    - 외래키 자신이 속한 릴레이션의 기본키를 참조하도록 외래키를 정의한 경우, 참조하는 릴레이션과 참조되는 릴레이션이 같을 수도 있다.
    - ex. 고객 릴레이션에 추천 고객 컬럼은 외래키인데 같은 릴레이션의 기본키를 참조한다.
      - 이것은 고객 릴레이션이 자신과 관계를 맺고 있음을 의미한다.
  - 외래키가 되는 속성과 참조되는 기본키가 되는 속성의 이름은 달라도 되지만, 도메인은 반드시 같아야 한다.
    - 도메인이 같아야 연관성 있는 튜플을 찾기 위한 비교 연산이 가능하기 때문
  - 외래키는 기본키를 참조하지만 기본키가 아니기 때문에 널 값을 가질 수 있고, 서로 다른 튜플이 같은 값을 가질 수 있다.

### 무결성 제약조건
- 관계 데이터 모델에서 정의하고 있는 기본 제약 사항은 키와 관련한 무결성 제약조건이다.
  - 개체 무결성 제약조건
    - 기본키를 구성하는 모든 속성은 널 값을 가질 수 없다.
    - 이를 만족시키기 위해 DBMS는 삽입, 변경 연산이 발생할 때, 기본키에 널 값이 포함되는 상황에서는 연산의 수행을 거부한다.
  - 참조 무결성 제약조건
    - 외래키는 참조할 수 없는 값을 가질 수 없다.
    - 외래키가 널 값이더라도 참조 무결성 제약조건을 위반한 것은 아니다.
