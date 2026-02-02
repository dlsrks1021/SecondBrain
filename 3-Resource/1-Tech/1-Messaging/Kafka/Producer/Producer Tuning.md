
Kafka Producer는 메시지 전송을 여러 단계로 나누어 관리하며,  
각 단계는 성능과 메모리 사용량에 직접적인 영향을 준다.

send → batching → await send → retries → inflight

---

## max.block.ms

send() 호출 시  
Producer가 얼마나 오래 블록될 수 있는지를 결정한다.

다음 상황에서 send()는 블록될 수 있다.

- 메타데이터가 아직 준비되지 않은 경우
- 전송 버퍼가 가득 찬 경우

설정된 시간 내에 문제가 해결되지 않으면 예외가 발생한다.

이 예외는 Future가 아니라 send() 호출 시점에 발생한다.

---

## linger.ms

현재 배치를 전송하기 전까지  
얼마나 대기할지를 결정한다.

- 배치가 가득 차면 즉시 전송
- 가득 차지 않아도 linger.ms 시간이 지나면 전송

지연 시간을 약간 늘리는 대신 처리량을 크게 증가시킬 수 있다.

---

## retries / retry.backoff.ms

- retries: 재시도 횟수
- retry.backoff.ms: 재시도 간 대기 시간

일시적인 오류 상황에서 재전송을 제어한다.

---

## request.timeout.ms

Producer가 브로커에 요청을 보낸 뒤  
응답을 기다리는 최대 시간이다.

해당 시간이 초과되면 요청은 실패로 간주된다.

---

## delivery.timeout.ms

레코드 전송 준비가 완료된 시점부터  
성공 또는 최종 실패까지의 전체 제한 시간이다.

linger.ms와 request.timeout.ms의 합보다 커야 한다.

---

## buffer.memory

Producer가 메시지를 전송하기 전에  
메모리에 대기시킬 수 있는 전체 버퍼 크기이다.

버퍼가 가득 차면 send()는 max.block.ms 동안 블록되며,  
이후 예외가 발생한다.
