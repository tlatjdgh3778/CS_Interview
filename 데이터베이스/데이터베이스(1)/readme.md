# 데이터베이스란?
데이터베이스는 여러 사람이 공유하여 사용할 목적으로 체계화해 통합, 관리하는 데이터의 집합니다.

# 1. 인덱스
인덱스(index)란 데이터베이스에서 테이블에 대한 동작의 속도를 높여주는 자료 구조를 일컫는다. 인덱스는 말 그대로 책의 맨 처음 또는 마지막에 있는 색인이라고 할 수 있다. 책에서 원하는 내용을 찾을 때 색인을 이용해서 찾으면 더욱 쉽게 찾을 수가 있다.

이처럼 데이터베이스에서도 테이블의 모든 데이터를 검색하면 시간이 오래 걸리기 때문에 칼럼과 해당 레코드가 저장된 주소를 키/값 쌍으로 인덱스를 만들어 두는 것이다.

자주 사용되는 컬럼에 대해 index 테이블을 따로 만들어 SELECT 문이 들어왔을 때 index 테이블에 있는 값들로 결과 값을 조회해 온다. index를 잘 활용 한다면 검색 연산을 실행했을 때 성능을 올릴 수 있게 된다.

<p align="center"><img src="https://github.com/WeareSoft/tech-interview/raw/master/contents/images/db-index.png"></p>

* index 테이블에서 where 절에 포함된 값을 검색
* 해당 값의 table_id PK를 획득
* 가져온 table_id PK 값으로 원본 테이블에서 값을 조회

## 1-1. 인덱스의 장점과 단점
**장점**
* 테이블을 조회하는 속도와 성능을 향상시킬 수 있다.
* 시스템의 부하를 줄일 수 있다.

**단점**
* 인덱스를 관리하기 위해 인덱스 테이블의 형태로 관리하는데 DB의 약 10%에 해당하는 저장공간을 사용한다.
* 인덱스를 관리하기 위해 추가 작업이 필요하다. 
* 인덱스를 잘못 사용하면 오히려 성능이 저하될 수 있다.
  * INSERT : 테이블에는 입력 순서대로 저장되지만 인덱스 테이블은 정렬되어 저장하기 때문에 성능 저하가 발생한다.
  * DELETE : 테이블에서만 삭제되고 인덱스 테이블에서는 "사용하지 않음" 처리만 해주고 남아있기 때문에 쿼리 수행 속도가 저하된다.
  * UPDATE : DELETE연산과 INSERT연산을 수행하여 UPDATE를 하기 때문에 성능 저하가 발생한다.

## 1-2. 인덱스의 자료구조
인덱스를 구현하기 위해서 대표적으로 **해시 테이블**과 **B+Tree** 자료구조를 사용한다.

### 해시 테이블(Hash Table)
해시 테이블은 키(key)와 값(value)으로 데이터를 저장하는 자료구조이다.

해시 테이블은 검색하고자 하는 key를 입력 받아서 해시 함수를 적용하고 반환 받은 해시 코드를 배열의 index로 환산을 해서 해당 index에 저장된 값에 접근한다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FceKgGz%2FbtqAUvLYrPN%2FDMVl0lwN8tA2hobFxqHcf0%2Fimg.png"></p>

기본적으로 해시 테이블의 시간복잡도는 **O(1)** 이어서 빠른 검색을 지원한다.

하지만 데이터베이스에서 해시 테이블이 사용되는 경우는 제한적인데 그 이유는 해시가 등호 연산(=)에만 특화되어있기 때문이다. 해시 함수는 값이 조금만 달라도 완전히 다른 해시 값을 생성하기 때문에 부등호 연산(<,>)이 자주 사용되는 데이터베이스 검색을 위해서는 해시 테이블이 적합하지 않다.

### B+Tree
B+Tree는 자식 노드가 2개 이상이 가능한 B-Tree의 확장개념으로, B-Tree의 경우는 branch node에도 key와 data를 담을 수 있다. 하지만 B+Tree의 경우에는 leaf node에만 key와 data를 저장한다.

B+Tree는 leaf node를 제외하고 data를 담아두지 않기 때문에 메모리를 확보함으로써 더 많은 key들을 수용할 수 있다. leaf node는 LinkedList로 연결되어 있다.

full scan을 할 때에 B+Tree의 경우는 leaf node에 data가 모두 있기 때문에 한 번의 선형 탐색만 해주면 된다. B-Tree의 경우에는 모든 노드를 확인해야 한다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Bplustree.png/1024px-Bplustree.png"></p>

# 2. 정규화
정규화란 데이터베이스에서 중복을 최소화하기 위해 데이터를 구조화하는 프로세스를 말한다. 정규화를 통해 불필요한 데이터를 없앨 수 있고 각종 이상 현상들을 방지할 수 있다.

## 2-1. 이상 현상
이상 현상(Anomaly)이란 테이블 내의 데이터 중복성에 의해서 발생되는 데이터 불일치 현상을 말한다.

이상 현상은 갱신 이상, 삽입 이상, 삭제 이상이 있다.

### 갱신 이상(Modification Anomaly)
중복된 튜플 중 일부의 속성만 갱신시킴으로써 정보의 모순성이 발생하는 경우이다.

Employee ID가 519번인 직원의 주소를 변경했을 때 그 직원의 모든 주소에 대해 변경이 이루어져야 하는데 일부만 갱신이 됐을 경우이다. 이 경우 519번의 주소가 무엇인가에 대한 모순이 생긴다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/1/12/Update_anomaly.png"></p>

### 삽입 이상(Insertion Anomaly)
데이터를 저장할 때 원하지 않는 정보가 함께 저장되는 경우이다.

새로운 교수를 고용하였을 때 해당 교수는 아직 맡은 강의가 없기 때문에 Course Code를 null로 하지 않는 이상 테이블에 추가할 수 없다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Insertion_anomaly.svg/1024px-Insertion_anomaly.svg.png"></p>

### 삭제 이상(Deletion Anomaly)
튜프를 삭제할 때 유지되어야 하는 정보까지도 연쇄적으로 삭제되는 경우이다.

한 교수의 강의를 임시 중단하고자 할 때에 기록된 레코드를 삭제해야하는 그 경우 교수 자체가 사라지는 현상이 발생한다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2c/Deletion_anomaly.svg/1024px-Deletion_anomaly.svg.png"></p>

## 2-2. 정규화 과정

### 제 1정규형(1NF)
릴레이션에 속한 모든 속성의 값이 원자값이면 1정규형에 속한다.

여러 개의 값을 가진 취미를 원자값을 가지도록 분해한다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/233FF547596B5C8830"></p>

### 제 2정규형(2NF)
제 2정규형은 제1 정규형을 만족하고, 기본키가 아닌 속성이 기본키에 완전 함수 종속을 만족해야 한다. 완전 함수 종속은 기본키의 부분집합이 결정자가 되어서는 안 된다는 뜻이다.

이 테이블에서 **(학생번호, 강좌이름)** 인 기본키가 **강의실**을 결정하고 있다. (학생번호, 강좌이름) -> (강의실)

그리고 기본키의 부분집합인 **강좌이름**이 **강의실**을 결정할 수 있다. (강좌이름) -> (강의실)

기본키의 부분집합인 **강좌이름**이 **강의실**을 결정하고 있으므로(결정자) 완전 함수 종속을 만족하지 않는다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/22259D36596B5DB818"></p>

아래의 그림과 같이 강의실 테이블로 분해함으로써 완전 함수 종속을 만족해준다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/2577A136596B5DB802"></p>

### 제 3정규형(3NF)
제3 정규형은 제2 정규형을 만족하고, 기본키가 아닌 모든 속성이 기본키에 이행적 함수 종속이 되지 않아야 한다. 이행적 종속이란 A -> B, B -> C 일 때 A -> C가 성립되는 것을 말한다.

**학생번호**가 **강좌이름**을 결정하고 있고, **강좌이름**이 **수강료**를 결정하고 있는 테이블이다. (학생번호) -> (강좌이름), (강좌이름) -> (수강료)

따라서 (학생번호) -> (수강료)를 만족하므로 이행적 함수 종속인 상태이다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/22042D38596B606610"></p>

아래와 같이 이행적 함수 종속을 없앰으로써 제3 정규형으로 만들어 준다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/223DD238596B606621"></p>

### BCNF 정규형
BCNF 정규형은 제3 정규형을 만족하고, 모든 결정자가 후보키가 되도록 테이블을 분해하는 것이다.

* 후보키 : 튜플을 유일하게 식별할 수 있는 속성들의 부분집합. 유일성과 최소성을 만족해야 한다.

기본키는 **(학생번호, 특강이름)** 이고, 기본키가 **교수**를 결정하고 있다. 그리고 **교수**는 **특강이름**을 결정하고 있다.

여기서 **교수**는 **특강이름**을 결정하는 결정자이지만 후보키는 아니기 때문에 BCNF 정규형을 만족하지 않는다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/25B45A33596B623C0E"></p>

아래와 같이 분해하면 모든 결정자가 후보키임을 만족해서 BNCF 정규형을 만족한다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/276B8F33596B623C3A"></p>


# 3. 트랜잭션
트랜잭션(Transaction)은 작업의 완전성을 보장해주는 것이다. 논리적인 작업 셋을 모두 완벽하게 처리하거나 또는 처리하지 못할 경우에는 원래의 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어주느 기능이다.

사용자의 입장에서는 작업의 논리적 단위로 이해를 할 수 있고 시스템의 입장에서는 데이터들을 접근 또는 변경하는 프로그램 단위가 된다.

## 3-1. 트랜잭션의 특징
ACID(Atomicity, Consistency, Isolation, Durability)는 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 특징을 가리키는 약어이다.

 * **원자성(Atomicity)**
   * 트랜잭션의 연산은 데이터베이스에 모두 반영되거나 아니면 전혀 반영되지 않아야 한다.(All or Nothing)
 * **일관성(Consistency)**
   * 트랜잭션이 성공적으로 완료되면 일관적인 DB상태를 유지하는 것을 말한다.
   * 트랜잭션이 완료된 다음의 상태에서도 트랜잭션이 일어나기 전의 상황과 동일하게 데이터의 일관성을 보장해야 하는 것을 말한다.
 * **고립성(Isolation)**
   * 트랜잭션 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하는 것을 말한다.
   * 수행중인 트랜잭션은 완전히 완료될 때까지 다른 트랜잭션에서 수행 결과를 참조할 수 없다.
 * **지속성, 영속성(Durability)**
   * 성공적으로 수행된 트랜잭션은 영원히 반영되어야 함을 의미한다.
   * 시스템 문제, DB 일관성 체크 등을 하더라도 유지되어야 한다.

## 3-2. 트랜잭션의 연산과 상태
### Commit 연산
Commit 연산은 한 개의 트랜잭션에 대한 작업이 성공적으로 끝났고 데이터베이스가 일관된 상태에 있을 때 이 트랜잭션이 행한 갱신 연산이 완료된 것을 트랜잭션 관리자에게 알려주는 연산이다.

### Rollback 연산
Rollback 연산은 하나의 트랜잭션 처리가 비정상적으로 종료되어 데이터베이스의 일관성을 깨뜨렸을 때, 이 트랜잭션의 일부가 정상적으러 처리되었더라도 트랜잭션의 원자성을 보장하기 위해 이 트랜잭션이 행한 모든 연산을 취소(Undo)하는 연산이다.

Rollback시에는 해당 트랜잭션은 재시작하거나 폐기한다.

### 트랜잭션의 상태

<p align="center"><img src="https://media.vlpt.us/images/issac/post/1e2e072c-01c0-405e-9bfc-1be6ce5f831d/image.png"></p>

* **활성(Active)**
  * 트랜잭션이 정상적으로 실행중인 상태
* **실패(Failed)**
  * 트랜잭션 실행에 오류가 발생하여 중단된 상태
* **철회(Aborted)**
  * 트랜잭션이 비정상적으로 종료되어 Rollback 연산ㅇ르 수행한 상태
* **부분 완료(Partially Committed)**
  * 트랜잭션의 마지막 연산까지 실행했지만, Commit 연산이 실행되기 직전의 상태
* **완료(Committed)**
  * 트랜잭션이 성공적으로 종료되어 Commit 연산을 실행한 후의 상태

# 참고
* https://mangkyu.tistory.com/96
* [위키 - B+Tree](https://ko.wikipedia.org/wiki/B%2B_%ED%8A%B8%EB%A6%AC)
* [위키 - 정규화](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%A0%95%EA%B7%9C%ED%99%94)
* https://chokyuhwan.tistory.com/27
* [트랜잭션의 상태](https://velog.io/@issac/DB-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-Transaction%EC%9D%98-ACID-%EC%86%8D%EC%84%B1%EA%B3%BC-%EB%B6%84%EC%82%B0%EC%8B%9C%EC%8A%A4%ED%85%9C-BASE-%EC%86%8D%EC%84%B1)
