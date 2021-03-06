# Key

- Key란? 검색이나 정렬 시 Tuple을 구분할 수 있는 기준이 되는 Attribute

### 1. Candidate Key(후보키)

- 릴레이션을 구성하는 속성들 중에서 **Tuple을 유일하게 식별할 수 있는 속성들의 부분 집합**을 의미한다.(기본키로 사용할 수 있는 속성들을 후보키라 한다.)
- **모든 릴레이션은 반드시 하나 이상의 후보 키를 가져야 한다.**
- 릴레이션에 있는 모든 튜플에 대해서 **유일성과 최소성을 만족**시켜야 한다.

> 유일성: key로 하나의 Tuple을 유일하게 식별할 수 있음
> 최소성: 꼭 필요한 속성으로만 구성

즉, 기본키가 될 수 있는 키들을 후보키라 한다.

### 2. Primary Key(기본키)

- 후보 키 중 선택한 Main key
- 한 릴레이션에서 **특정 튜플을 유일하게 구별할 수 있는 속성**
- **Null 값을 가질 수 없다.(개체 무결성의 첫 번째 조건)**
- **기본 키로 정의된 속성에는 동일한 값이 중복되어 저장될 수 없다.(개체 무결성의 두번째 조건)**

> 유일성: 기본키를 구성하는 컬럼은 테이블에서 레코드를 식별할 수 있도록 유일해야 한다.
> 최소성: 유일성을 만족하는 한도 내에서 최소한의 컬럼(하나 이상)으로 구성되어야 한다.
> 개체 무결성: 기본키가 가지고 있는 값의 유일성을 보장받아야 한다.

### 3. Alternate Key(대체키)

- 후보키가 둘 이상일 때, 기본 키를 제외한 나머지 후보키들을 말하낟.
- 보조키라고도 한다.

### 4. Super Key(슈퍼키)

- **한 릴레이션 내에 있는 속성들의 집합으로 구성된 키**로서 릴레이션을 구성하는 모든 튜플 중 슈퍼키로 구성된 속성의 집합과 동일한 값을 나타내지 않는다.
- 릴레이션을 구성하는 모든 튜플에 대해서 **유일성은 만족하지만, 최조성은 만족시키지 못한다.**

### 5. Foreign Key(외래키)

- 관계(Relation)를 맺고 있는 릴레이션 R1, R2에서 릴레이션 R1이 참조하고 있는 릴레이션 R2의 기본키와 같은 R1 릴레이션의 속성을 외래키라고 한다.
- 외래키는 **참조되는 릴레이션의 기본키와 대응되어 릴레이션간에 참조관계를 표현하는데 중요한 도구로 사용된다.**
- **외래키로 지정되면 참조 테이블의 기본키에 없는 값은 입력할 수 없다.**(참조 무결성의 조건)
