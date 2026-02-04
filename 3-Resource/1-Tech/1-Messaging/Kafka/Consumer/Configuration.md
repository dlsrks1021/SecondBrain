
컨슈머 설정은 처리량, 지연, 안정성에 직접적인 영향을 준다.

---

## Fetch 관련

fetch.min.bytes (default: 1)  
브로커가 응답하기 위한 최소 데이터 크기

fetch.max.wait.ms (default: 500)  
fetch.min.bytes 충족을 위해 대기하는 최대 시간

fetch.max.bytes (default: 50MB)  
poll() 호출 시 브로커가 반환할 최대 바이트 수

max.partition.fetch.bytes (default: 1MB)  
파티션별 최대 반환 크기

---

## Poll / Heartbeat

max.poll.records (default: 500)  
poll() 호출당 최대 레코드 수

max.poll.interval.ms (default: 5m)  
poll 미호출 허용 최대 시간
타임아웃 발생 시:
- leave group 요청 전송
- 하트비트 전송 중단

session.timeout.ms (default: 45s)  
컨슈머 생존 판정 기준

heartbeat.interval.ms (default: 3s)  
그룹 코디네이터로의 하트비트 전송 주기 (백그라운드 동작)

---

## Offset

auto.offset.reset (default: latest)  
유효 오프셋 없을 경우 레코드를 읽는 기준
- latest: 컨슈머 작동 시점 다음부터 쓰여진 레코드부터 읽기
- earliest: 파티션의 맨 처음부터 모든 데이터를 읽기
- none: 유효 오프셋 없으면 예외 발생

enable.auto.commit (default: true)
자동 커밋 활성 여부

auto.commit.interval.ms (default: 5s)
자동 커밋의 주기

---

## 파티션 할당 전략

partition.assignment.strategy

- Range  
  토픽별 연속 파티션 할당  
  파티션 불균형 발생 가능

- RoundRobin  
  전체 파티션을 순차적으로 분배  
  균등 분배

- Sticky  
  기존 할당 유지 우선  
  Rebalance 비용 감소

- CooperativeSticky  
  Sticky + Cooperative Rebalance 지원  
  Kafka 3.x 기본 전략

---

## 기타

client.id  
클라이언트 식별자 (로깅, 메트릭)

client.rack
broker.rack과 같은 레플리카를 우선 읽도록 설정 가능

group.instance.id  
정적 그룹 멤버십 활성화

receive.buffer.bytes / send.buffer.bytes  
TCP 송수신 버퍼 크기

(offsets.retention.minutes)
브로커의 설정
커밋된 오프셋의 보관 기간
