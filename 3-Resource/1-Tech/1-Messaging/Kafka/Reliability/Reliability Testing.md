신뢰성은 설정만으로 보장되지 않는다.
반드시 검증이 필요하다.

---

### 설정 검증

- VerifiableProducer / VerifiableConsumer 사용
- 리더 선출 시 복구 시간 측정
- 컨트롤러 재시작 테스트
- 롤링 재시작 중 메시지 유실 여부
- 언클린 리더 선출 허용 여부 평가

---

### 애플리케이션 검증

- 브로커 연결 단절
- 네트워크 지연
- 디스크 Full / Disk Failure
- 리더 선출
- 프로듀서 / 컨슈머 롤링 재시작

Trogdor 테스트 프레임워크를 활용할 수 있다.

---

### 프로덕션 모니터링 주요 지표

프로듀서
- error-rate
- retry-rate

컨슈머
- consumer lag
- Burrow 활용

브로커
- FailedProduceRequestsPerSec
- FailedFetchRequestsPerSec
