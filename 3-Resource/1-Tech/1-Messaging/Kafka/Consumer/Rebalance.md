
리밸런스는 컨슈머 그룹 내에서
파티션 소유권을 재분배하는 과정이다.

---

## 리밸런스 발생 조건

- 컨슈머 추가
- 컨슈머 종료 또는 크래시
- 구독 토픽 변경
- 파티션 수 변경

---

## Eager Rebalance

- 모든 컨슈머 중단
- 모든 파티션 반납
- 전체 재할당

### 단점
- Stop-the-world 발생
- 그룹이 클수록 지연 증가

---

## Cooperative Rebalance

- 이동 대상 파티션만 재할당
- 나머지 파티션은 계속 처리
- 여러 단계에 걸쳐 수행

Kafka 3.1 이후 기본값이다.

---

## Rebalance Listener

컨슈머는 리밸런스 중
정리 작업을 수행할 수 있다.

subscribe() 시 ConsumerRebalanceListener를 등록한다.

### onPartitionsRevoked
- 파티션이 해제되기 직전 호출
- Offset Commit 수행 위치
- 리소스 정리

### onPartitionsAssigned
- 파티션 재할당 후 호출
- seek, 상태 복구 수행

### onPartitionsLost
- Cooperative Rebalance에서
  파티션이 예외적으로 상실된 경우 호출
