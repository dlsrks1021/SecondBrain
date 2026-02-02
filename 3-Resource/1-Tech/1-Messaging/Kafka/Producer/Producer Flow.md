
Kafka Producer의 메시지 전송은 여러 단계를 거쳐 수행된다.

---

## ProducerRecord 생성

Kafka에 메시지를 쓰는 작업은 ProducerRecord 객체 생성으로 시작된다.

- Topic과 Value는 필수
- Key와 Partition은 선택

---

## 직렬화

Producer는 Key와 Value 객체를 네트워크 전송이 가능하도록  
[[Serializer]]를 이용해 byte array로 변환한다.

---

## Partitioner

Partition이 명시되지 않은 경우  
레코드는 [[Partitioner]]로 전달된다.

---
## Record Batch

Partition이 결정되면 레코드는  
동일 Topic-Partition 단위의 Record Batch에 추가된다.

Producer 내부의 Sender Thread는  
배치가 가득 차거나 일정 시간이 지나면 배치를 전송한다.

---

## Broker 응답

브로커가 메시지를 성공적으로 저장하면  
다음 정보를 담은 RecordMetadata를 반환한다.

- Topic
- Partition
- Offset

메시지 저장에 실패한 경우 에러가 반환되며,  
에러 유형에 따라 재시도 여부가 결정된다.
