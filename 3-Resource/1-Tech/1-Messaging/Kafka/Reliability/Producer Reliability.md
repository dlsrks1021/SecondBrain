
Kafka Producer의 신뢰성은  
주로 acks와 재시도 관련 설정에 의해 결정된다.

---

## acks

프로듀서가 메시지 전송을 성공으로 판단하기 위해  
얼마나 많은 레플리카의 응답을 기다릴지를 결정한다.

### acks=0
- 브로커 응답을 기다리지 않는다
- 가장 빠르지만 메시지 유실 가능성이 있다

### acks=1
- 리더 레플리카 수신 시 성공으로 판단
- 복제 전에 리더 장애 발생 시 메시지 유실 가능
	- Leader not Available 에러 발생

### acks=all
- min.insync.replicas 만큼 ISR에 복제가 완료된 뒤 성공
- 가장 안정적이지만 지연 시간이 증가한다

카프카는 모든 ISR에 복제가 완료된 메시지만 컨슈머가 읽을 수 있으므로,  
end-to-end 지연 시간은 acks 값과 무관하다.

---

## 재시도 설정

- retries: 재시도 횟수
- retry.backoff.ms: 재시도 간 대기 시간

일시적인 장애 상황에서 브로커 부하를 줄이기 위해 사용된다.

메시지 유실을 막기 위한 가장 안전한 전략은 다음과 같다.

- retries 기본값 유지 (MAX_INT)
- delivery.timeout.ms를 충분히 크게 설정
- enable.idempotence=true 설정

재시도는 중복을 발생시킬 수 있으므로
멱등성 프로듀서 설정이 중요하다.

### 추가적인 에러 처리 대상

- 메시지 크기 초과, 인가 오류
- 직렬화 오류
- 재시도 소진
- 프로듀서 메모리 부족
- 타임아웃

단순 재전송만 필요하다면
프로듀서의 내장 재시도 메커니즘이 가장 안전하다.

---

## 멱등성 프로듀서

enable.idempotence를 활성화하면  
Producer는 레코드마다 순차적인 번호를 부여한다.

브로커는 동일한 번호의 레코드를 중복 저장하지 않는다.

설정 조건:
- acks=all
- retries >= 1
- max.in.flight.requests.per.connection <= 5
