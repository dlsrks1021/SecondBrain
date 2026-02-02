
Kafka Producer의 신뢰성은  
주로 acks와 재시도 관련 설정에 의해 결정된다.

---

## acks

acks는 Producer가 메시지 전송을 성공으로 판단하기 위해  
얼마나 많은 레플리카의 응답을 기다릴지를 결정한다.

### acks=0
- 브로커 응답을 기다리지 않는다
- 가장 빠르지만 메시지 유실 가능성이 있다

### acks=1
- Leader 레플리카 수신 시 성공으로 판단
- 복제 전에 Leader 장애 발생 시 메시지 유실 가능

### acks=all
- 모든 in-sync replica에 복제가 완료된 뒤 성공
- 가장 안정적이지만 지연 시간이 증가한다

Kafka는 모든 ISR에 복제가 완료된 메시지만 Consumer가 읽을 수 있으므로,  
end-to-end 지연 시간은 acks 값과 무관하다.

---

## 재시도 설정

- retries: 재시도 횟수
- retry.backoff.ms: 재시도 간 대기 시간

일시적인 장애 상황에서 브로커 부하를 줄이기 위해 사용된다.

---

## Idempotent Producer

enable.idempotence를 활성화하면  
Producer는 레코드마다 순차적인 번호를 부여한다.

브로커는 동일한 번호의 레코드를 중복 저장하지 않는다.

설정 조건:
- acks=all
- retries >= 1
- max.in.flight.requests.per.connection <= 5
