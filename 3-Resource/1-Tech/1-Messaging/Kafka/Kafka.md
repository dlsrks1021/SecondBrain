Kafka는 **메시지 발행/구독(Pub/Sub) 시스템**으로,  
메시지를 **순서를 유지한 채 디스크에 지속성 있게 저장**하며  
Consumer는 이를 **결정적(deterministic)** 으로 읽을 수 있다.

Kafka는 단순 메시징 시스템을 넘어  
**대규모 이벤트 스트리밍 플랫폼**으로 사용된다.

---

## Message & Stream

Kafka에서 데이터의 기본 단위는 **Message**이다.

- 메시지는 optional한 **Key**를 가질 수 있다.
- Key는 메시지가 저장될 **Partition을 결정**한다.
- 같은 Key를 가진 메시지는 항상 **같은 Partition**에 저장된다.

하나의 Topic에 저장된 메시지 흐름을 **Stream**이라고 하며,  
이는 **Producer → Consumer로 전달되는 데이터 흐름**을 의미한다.

→ [[Message]]

---

## Schema

Kafka 입장에서 메시지는 단순한 **byte array**이지만,  
운영 및 확장성을 위해 **명확한 스키마 정의**가 권장된다.

- 간단한 방식: JSON, XML
- 권장 방식: [[Avro]]
  - 타입 안정성
  - 스키마 버전 호환
  - Schema Registry와의 연계

→ [[Schema]]

---

## Topic & Partition

Kafka에 저장되는 메시지는 **Topic 단위**로 분류된다.  
Topic은 다시 **여러 Partition**으로 나뉜다.

- Partition 내부에서는 **메시지 순서가 보장**
- Topic 전체 단위에서는 **순서 보장 X**
- Partition은 Kafka의 **병렬 처리 단위**

→ [[Topic & Partition]]

---

## Producer & Consumer

### Producer
- Topic에 메시지를 발행
- [[Partitioner]]를 통해 Partition 결정
- 메시지는 **Partition Leader**에게 전송됨

→ [[Producer]]

### Consumer
- 하나 이상의 Topic 구독
- Partition 단위로 메시지를 순서대로 읽음
- [[Consumer Group]] 단위로 동작

→ [[Consumer]]

---

## Broker & Cluster

Kafka 서버 하나를 **Broker**라고 한다.

Broker의 역할:
- 메시지 수신 및 **Offset 할당**
- 디스크에 메시지 저장
- Consumer의 Fetch 요청 처리

Kafka는 **Cluster 기반 구조**로 동작한다.

- 하나의 Broker가 **Controller** 역할 수행
- Partition은 하나의 **Leader Broker**를 가짐
- 복제된 Partition은 **Follower Broker**

→ 장애 발생 시 Follower가 Leader로 승격됨

→ 자세한 내용:
- [[Broker]]
- [[Controller]]
- [[Replication]]

---

## Kafka Ecosystem

- [[MirrorMaker]]: 클러스터 간 데이터 복제
- [[Schema Registry]]: 스키마 관리
- [[Kafka Connect]]: 외부 시스템 연동
- [[Kafka Streams]]: 스트림 처리

---

## Why Kafka?

- 다중 Producer / 다중 Consumer 지원
- Consumer 간 **상호 간섭 없음**
- 디스크 기반 메시지 보존
- 수평 확장 가능한 구조
- 대규모 트래픽 처리에 적합
