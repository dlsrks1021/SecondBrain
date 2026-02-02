
Kafka Producer는 메시지를 Kafka Topic에 발행하는 클라이언트이다.  
Kafka는 다양한 사용 사례를 가지며, 그만큼 Producer에 대한 요구 조건도 다양하다.

- 모든 메시지가 중요해 유실이 허용되지 않는 경우
- 일부 메시지는 유실되더라도 처리량이 중요한 경우
- 중복은 허용되지만 지연 시간이나 순서가 중요한 경우

Kafka Producer는 이러한 요구 조건을 설정값 조합을 통해 조절할 수 있도록 설계되어 있다.

---

## Producer의 메시지 전송 흐름

Kafka에 메시지를 쓰는 작업은 ProducerRecord 객체를 생성하는 것으로 시작된다.

1. ProducerRecord 생성
2. Key / Value 직렬화 ([[Serializer]])
3. Partition 결정 ([[Partitioner]])
4. 동일 Topic-Partition 단위로 Record Batch 적재
5. Sender Thread가 Broker로 전송
6. Broker 응답 수신 또는 에러 처리

이 전체 과정은 비동기적으로 수행된다.

→ 내부 동작 상세: [[Producer Flow]]

---

## 필수 설정 값

KafkaProducer 객체를 생성하기 위해 반드시 필요한 설정은 다음 세 가지이다.

### bootstrap.servers

Producer가 Kafka 클러스터와 최초 연결을 생성하기 위해 사용하는  
브로커의 host:port 목록이다.

- 모든 브로커를 포함할 필요는 없다
- 다만 일부 브로커 장애 상황에서도 연결을 유지하기 위해 2개 이상 지정하는 것이 권장된다

Producer는 최초 연결 이후 클러스터 메타데이터를 주기적으로 갱신한다.

### key.serializer

메시지의 Key 객체를 byte array로 변환하기 위한 Serializer 클래스이다.

Kafka Broker는 메시지를 byte array로만 처리하지만,  
Producer API는 임의의 객체를 Key로 사용할 수 있기 때문에  
Producer는 직렬화 방식을 알고 있어야 한다.

- org.apache.kafka.common.serialization.Serializer 인터페이스 구현체
- 기본 제공: StringSerializer, IntegerSerializer
- Key 없이 Value만 전송하는 경우에도 설정은 필요하며  
  이때는 VoidSerializer를 사용할 수 있다

### value.serializer

메시지의 Value 객체를 직렬화하기 위한 Serializer 클래스이다.

실제 서비스에서는 JSON보다는  
Avro와 Schema Registry 조합이 더 많이 사용된다.

→ [[Serializer]]

---

## 메시지 전송 방식

Kafka Producer는 메시지 전송 방식에 따라 세 가지 패턴으로 사용할 수 있다.

### Fire-and-forget

메시지를 브로커로 전송만 하고  
성공 여부나 실패 여부를 전혀 확인하지 않는 방식이다.

- Producer는 내부적으로 재시도를 수행한다
- 재시도 불가능한 에러나 타임아웃 발생 시 메시지는 유실될 수 있다
- 애플리케이션은 실패 사실을 알 수 없다

가장 빠른 방식이며, 로그나 통계 데이터 전송에 적합하다.

### Synchronous send

send() 호출 이후 Future의 완료를 기다려  
브로커의 응답을 확인하는 방식이다.

- 메시지 전송 성공 여부를 즉시 확인 가능
- 처리량 감소
- 지연 시간 증가

메시지 유실이 치명적인 경우에 사용된다.

### Asynchronous send (Callback)

메시지를 비동기로 전송하되,  
브로커 응답 시점에 콜백을 통해 성공 또는 실패를 처리한다.

- 성능과 신뢰성의 균형이 좋다
- 대부분의 실서비스에서 가장 일반적으로 사용된다

---

## 에러 유형

Kafka Producer에서 발생하는 에러는 두 가지로 구분할 수 있다.

### 재시도 가능한 에러

일시적인 장애로 인해 발생하며,  
메시지를 다시 전송함으로써 해결될 수 있다.

- 네트워크 오류
- 파티션 리더 변경(NotLeaderForPartition)

설정된 재시도 횟수를 모두 소진한 뒤에만 예외가 발생한다.

### 재시도 불가능한 에러

재전송으로 해결되지 않는 오류이다.

- 메시지 크기 초과
- 직렬화 실패

재시도 없이 즉시 예외가 발생한다.

---

## 신뢰성과 성능 조절

Producer는 다음 설정을 통해  
신뢰성, 처리량, 지연 시간을 조절한다.

- acks
- retries
- enable.idempotence

→ 상세 설명: [[Producer Reliability]]  
→ 성능 튜닝: [[Producer Tuning]]
