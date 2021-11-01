# MongoDB 란

- MongoDB는 문서지향(Document-Oriented) 저장소를 제공하는 NoSQL 데이터베이스 시스템이다.
- 솔루션 자체적으로 분산 처리, 샤딩, 데이터 리밸런싱, 데이터 복제, 복구 등을 지원한다.
- Schema-Free(Schema-less)한 구조로 대용량의 데이터 작업에 효율적이다.
- BSON(Binary JSON) 형태로 각 문서가 저장되며 배열(Array)이나 날짜(Date)등 기존 RDMS에서 지원하지 않던 형태로도 저장할 수 있기 때문에 JOIN이 필요없이 한 문서에 좀 더 이해하기 쉬운 형태 그대로 정보를 저장할 수 있다는 것이 특징이다.
- 문서지향 데이터베이스로 객체지향 프로그래밍과 잘 맞고 JSON을 사용할 때 아주 유용합니다.

[MongoDB와 RDBMS 용어](https://www.notion.so/0ceaae8aa9014185a1452f32a73a40a3)

- MongoDB 클라이언트 프로그램에서 Cursor를 통해 반복적으로 실제 Document를 가져올 수 있다.
- MongoDB에서 쿼리의 결과를 커서로 반환하는 이유
  - 쿼리의 결과를 클라이언트 서버 메모리에 저장하지 않기 위해서다.
  - 필요할 때마다 지정된 Page Size 단위로 서버로부터 전송받아 MongoDB 클라이언트 서버에 캐싱한 후에 유저에게 서비스한다.

[구문 비교](https://www.notion.so/157e701bc44446a4a5342caf6ad63b30)

## 특징

- Document-oriented storage
  - Database > Collections > Documents 구조로 Document는 Key-Value형태의 BSON(Binary JSON)로 되어있다.
- Full Index Support
  - Single Field Indexes: 기본적인 인덱스 타입
  - Compound Indexes: RDBMS의 복합인덱스와 유사한 타입
  - Multikey Indexes: Array에 매칭되는 값이 하나라도 있으면 인덱스에 추가하는 인덱스
  - Geospatial Indexes and Queries: 위치기반 인덱스와 쿼리
  - Text Indexes: String에 인덱스 가능
  - Hashed Index: Btree 인덱스가 아닌 Hash 타입의 인덱스도 사용 가능
- Replication & High Availability: 데이터 복제 지원, 고 가용성
- Auto-Sharding: 자동으로 데이터를 분산 저장하며, 하나의 Collection처럼 사용할 수 있다. 수평적 확장 가능
- Fast In-Pace Updates: 고성능의 atomic operation을 지원
- Map/Reduce: 맵리듀스를 지원 (map과 reduce 함수의 조합으로 분산/병령 시스템 운용 지원, 하둡같은 MR전용 시스템에 비해서 성능은 떨어진다.)
- GridFS: 분산파일 자동 저장

## 장점

- Flexibility: Schema-less로 어떤 형태의 데이터도 저장 가능
- Performance: Read & Write 성능이 뛰어나다. 캐싱이나 많은 트래픽을 감당할 수 있다.
- Scalability: 스케일아웃 구조를 채택해서 쉽게 운용가능하다. Auto Sharding 지원
- Deep Query ability: 문서지향적 Query Language를 사용하여 SQL 만큼 강력한 Query성능을 제공
- Conversion / Mapping: JSON 형태로 저장이 가능해서 직관적이고 개발이 편리

## 단점

- Join이 없어 join이 필요 없도록 데이터 구조화 필요
- Memory Mapped File로 파일 엔진 DB, 메모리 관리를 OS에게 위임한다. 메모리에 의존적, 메모리 크기가 성능을 좌우한다
- SQL을 완전히 이전할 수 없다.

## NoSQL -CAP theorem

- NoSQL은 CAP 이론을 따른다.
- 분산 시스템에서 CAP 세 가지 속성을 모두 만족하는 것은 불가능하며, 2가지만 만족 할 수 있다는 것으로 정의할 수 있다.
- Consistency (일관성)
  - 모든 요청은 최신 데이터 또는 에러를 응답받는다.
    - DB가 3개로 분산되었다고 가정할 때, 하나의 특정 DB의 데이터가 수정되면 나미저 2개의 DB에서도 수정된 데이터를 응답 받아야 한다.
- Availability (가용성)
  - 모든 요청은 정상 응답을 받는다.
  - 특정 DB가 장애가 생겨도 서비스가 가능해야 한다.
- Partitions Tolerance (분리 내구성)
  - DB간 통신이 실패하는 경우라도 시스템은 정상 동작 한다.

## Physical 데이터 저장구조

- 메모리 크기에 민감하다.
- 기본적으로 memory mapped file을 사용한다.
- 데이터 write시 논리적으로 memory 공간에 write 후 bolck들을 주기적으로 디스크에 write한다.
- 이 디스크 쓰기 작업은 OS에 의해서 이루어 진다.
- OS에서 제공되는 가상 메모리를 사용
  - 가상 메모리는 Page라는 블럭 단위로 나뉘어지고 이 블럭 들을 디스크 블럭에 매핑되고 블럭들의 집합이 하나의 데이터 파일이 된다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5cf5817d-fadf-4fef-842e-4c84205e7ea9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5cf5817d-fadf-4fef-842e-4c84205e7ea9/Untitled.png)

- 메모리에 저장되는 내용은 데이터 블록과 인덱스가 저장된다.
- 물리 메모리에 해당 데이터 블록이 없다면 페이지 폴트가 발생하게 되고, 디스크에서 그 데이터 블록을 로드한다.
  - 페이지 폴드 발생 시 메모리와 디스크 사이에 스위칭으로 디스크 IO가 발생하고 성능 저하가 유발한다.

## Replica-Set

- Master-Slave 복제와 Replica-Set 두 가지 방식의 복제를 지원
- Master-Slave 복제는 3.2 버전 이후 부터 사용 권장하지 않는다.
- Replica-Set은 하나의 Primary와 하나 이상의 Secondary node로 구성
- Primary는 사용자의 모든 데이터 변경 요청을 처리하고 요청 내용을 로그에 기록을 하고 Secondary는 Primary의 로그를 가져와 동기화를 수행한다.
- 살아있는지 확인을 위해 노드는 서로 하트비트 메시지를 주고받는다.
- Primary node가 통신하지 않으면 새로운 Primary node를 선출해 서비스를 지속되도록 한다.
- MongoDB에서 데이터 변경은 Primary node에서 시작되고 Secondary 노드는 데이터 변경이 불가능하다. 하지만 조회는 가능하여 읽기 성능을 높히기 위해 확장이 가능하다.
- 아비터라는 노드가 존재하며 아비터 노드는 데이터를 가지고 있지 않고 문제 발생 시 Primary 선출을 위한 투표에 참가만 한다.
  - Primary에 문제가 발생시 과반수 투표로 Primary를 선출한다.
  - 과반수가 불가능할 경우 Primary가 선출되지 않는다.
  - 그럴때 아비터라는 노드가 투표에 참가하여 Primary를 선출한다.
- MongoDB 고가용성을 보장하기 위해 Replica-Set은 최소 3개로 구성한다.

## Sharding

- Sharding은 데이터를 여러 서버에 분산해서 저장하고 처리할 수 있도록 하는 기술로 분산 처리를 위한 솔루션이다.
- Sharding을 적용하기 위해 샤드 클러스터를 구축해야 한다.
  - Config Server 필요
    - 파티션된 데이터 범위와 샤드 위치 정보 등의 메타 정보를 저장
  - mongos (Router Server) 필요
    - 응용 프로그램이 필요한 데이터를 조회하거나 저장
    - 라우터는 쿼리 수행에 있어서 Proxy 역할을 하고 Config Server의 메타 정보를 참조하여 필요한 데이터가 저장된 샤드로 쿼리를 전달하고 그 결과를 다시 응용 프로그램으로 반환하는 역할을 한다.

## MongoDB 샤딩 아키텍쳐

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b83fb70e-1440-4b23-a434-9c513f3c14a6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b83fb70e-1440-4b23-a434-9c513f3c14a6/Untitled.png)

MongoDB 샤딩 아키텍쳐

- MongoDB 샤드 클러스터의 가장 중요한 3가지 컴포넌트는 샤드서버,  Config 서버, 라우터다.
- 샤드 서버는 Replica-Set 형태로 1개 이상 그리고 라우터 서버도 1개 이상 존재할 수 있다.
- Config 서버는 샤드 클러스터에 단 하나의 Replica-Set만 존재할 수 있다.
- Mongos(라우터서버)
  - 영구적인 데이터를 가지지 않는다.
  - 사용자의 쿼리 요청을 어떤 샤드로 전달할지 정하고, 각 샤드로부터 받은 쿼리 결과 데이터를 병합해서 사용자에게 반환한다.
  - 각 샤드가 균등하게 데이터를 가지고 있는지 모니터링하면서 데이터의 밸런싱 작업도 담당한다.
- 샤드 서버
  - 실제 사용자의 데이터를 저장한다.
- Config 서버
  - 샤드 서버에 저장된 사용자 데이터가 어떻게 split 되어서 분산돼 있는지에 관한 메타 정보를 저장한다.
- Config 서버와 샤드 서버는 모두 mongod 프로그램으로 실행되며 설정이 조금씩 다르다.

## 샤드 클러스터의 쿼리 수행 절차

- Config 서버는 모든 데이터베이스와 컬렉션의 목록을 관리하는 것이 아니라 샤딩이 활성화된 데이터베이스와 컬렉션의 정보만 관리한다.
- 샤딩이 활성화 되어 있지않은 객체들은 각 샤드서버가 로컬로 관리한다.
- 하나의 큰 컬렉션을 여러 조각으로 파티션하고 각 조각들을 여러 샤드 서버에 분산해서 저장한다. 이때 이런 각 데이터 조각을 MongoDB에서는 Chunk라고 한다.
- 쿼리 요청에 따른 클러스터 동작
  - 사용자 쿼리가 참조하는 컬렉션의 청크 메타 정보를 config 서버로부터 가져와서 라우터 메모리에 캐싱한다.
    - 청크 메타 정보가 없거나 오래된 경우 수행하고 매번 요청하지는 않는다.
  - 쿼리의 조건에서 샤딩 키 조건 찾는다.
    - 샤딩 키가 있으면 해당 샤딩 키가 포함된 청크 정보를 라우터의 캐시에서 검색하여 해당 샤드 서버에 요청한다.
    - 샤딩 키가 없으면 모든 샤드로 쿼리 요청한다.
  - 쿼리를 전송한 대상 샤드 서버로부터 쿼리 결과가 도착하면 결과를 병합하여 사용자에게 쿼리 결과로 커서를 반환한다.

## Config 서버

- 샤딩된 클러스터를 운영하는 데 있어서 필요한 모든 정보를 저장한다.
- 3대 이상으로 복제할 것을 권장한다.
- Replica-Set 구성 시 조건
  - Config Server는 반드시 WiredTiger 스토리지 엔진을 사용해야 한다.
  - 아비터 노드를 가질 수 없다.
  - 지연된 멤버를 가질 수 없다.
  - 최소 3개 이상의 멤버로 구성해야 한다.

## 라우터

- 사용자의 쿼리 요청을 샤드 서버로 전달하고, 샤드 서버로부터 쿼리 결과를 모아서 다시 사용자에게 반환하는 프록시 역할을 수행한다.
- 중요 역할
  - 사용자 쿼리를 전달해야 할 샤드 서버를 결정하고 해당 샤드로 쿼리 전송
  - 샤드 서버로부터 반환된 결과를 조합하여 사용자에게 결과 반환
  - 샤드간 청크 밸런싱 및 청크 스플릿 수행

## 라우터의 쿼리 분산

- 샤드 클러스터에서 컬렉션은 특정 필드의 값을 기준으로 샤딩될 수 있다.
- 사용자의 쿼리가 샤딩 기준 키 값에 대한 조건을 가지고 있는냐에 따라 라우터가 쿼리를 요청해야 할 샤드 서버를 결정하게 된다.
- Target Query
  - 라우터가 사용자의 쿼리를 특정 샤드로만 요청하는 형태
- Broadcast Query
  - 모든 샤드 서버로 요청하는 형태

## Storage Engine

- MMAP (Memory Mapping Storage-Engine)
  - 버전 4.2부터 지원 x
  - File Base 기반
- wiredTriger Storage-Engine
  - 버전 3.2 부터 기본 설정 스토리지
  - File Base 기반
  - 압축과 암호화 기능 제공
  - 다중 트랜잭션 중심의 데이터 처리에 적합
  - Point in time Recovery가 가능한 Dignostic 기능을 제공
- In-Memory Storage-Engine
  - Pure Memory 기반
  - 빠른 연산 처리 중심의 데이터 처리에 적합
  - 충분한 메모리 영역이 요구
  - Enterprise 버전 3.2.6부터 지원시작된 general-availability(GA)의 일부입니다.
  - 일부 메타 데이터 및 진단 데이터 이외에 구성 데이터, 인덱스, 사용자 자격 증명 등을 on-disk에 저장하지 않습니다.
  - 비영구적이며 스토리지에 데이터를 쓰지 않습니다.
  - Percona에서 MongoDB용 Percona Memory Engine을 Community Edition용 오픈소스 인메모리 엔진을 출시했다.

## Join 구문 예시

```bash
db.orders.insert([
	{ "_id" : 1, "item" : "abc", "price" : 12, "quantity" : 2 },
	{ "_id" : 2, "item" : "jkl", "price" : 20, "quantity" : 1 },
	{ "_id" : 3  }]);

db.inventory.insert([
	{ "_id" : 1, "sku" : "abc", description: "product 1", "instock" : 120 },
	{ "_id" : 2, "sku" : "def", description: "product 2", "instock" : 80 },
	{ "_id" : 3, "sku" : "ijk", description: "product 3", "instock" : 60 },
	{ "_id" : 4, "sku" : "jkl", description: "product 4", "instock" : 70 },
	{ "_id" : 5, "sku": null, description: "Incomplete" },
	{ "_id" : 6 }]);

db.orders.aggregate([
{ $lookup: {
		from: "inventory",
		localField: "item",
		foreignField: "sku",
		as: "inventory_docs"
	}
},
	{ $out : "coltest1" }
])
```

## Index

1. 기본 인덱스 _id
   - 모든 MongoDB의 컬렉션은 기본적으로 _id필드에 인덱스가 존재합니다.
   - 컬렉션을 만들 때 _id필드를 따로 지정하지 않으면 mongod 드라이버가 자동으로 _id 필드 값을 ObjectId로 설정해줍니다.
   - _id 인덱스는 unique합니다.
2. Single(단일) 필드 인덱스
   - _id 인덱스 외에 사용자가 지정한 필드 인덱스입니다.
3. Compound(복합) 필드 인덱스
   - 두 개 이상의 필드를 사용하는 인덱스를 복합 인덱스라 합니다.
4. Multikey 인덱스
   - 필드 타입이 배열인 필드에 인덱스를 적용 할 때는 Multikey 인덱스가 적용됩니다.
   - 이 인덱스를 통하여 배열에 특정 값이 포함되어 있는 document를 효율적으로 스캔 할 수 있습니다.
5. Geospatial(공간적) 인덱스
   - 지도의 좌표와 같은 데이터를 효율적으로 쿼리하기 위해서 사용되는 인덱스입니다.
6. Text 인덱스
   - 텍스트 관련 데이터를 효율적으로 쿼리하기 위한 인덱스입니다.
7. Hashed 인덱스
   - B Tree가 아닌 Hash 자료구조를 사용합니다.
   - 검색 효율이 B Tree보다 좋지만, 정렬을 하지 않습니다.

```bash
## 인덱스 생성
## 값을 1로하면 오름차순, -1로하면 내림차순으로 정렬합니다.
db.collection.createIndex( { KEY: 1} )

## 복합 필드 인덱스 예시
## age를 오름차순으로 정렬한 상태에서 score는 내림차순으로 정렬합니다.
db.report.createIndex( { age: 1, score: -1} )

## 인덱스 조회
db.collection.getIndexes()

## 인덱스 제거
db.collection.dropIndex( { KEY: 1} )
```