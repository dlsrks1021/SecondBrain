
Partitioner는 메시지가 저장될 Partition을 결정한다.

---

## 기본 Partitioner 동작

### Key가 있는 경우

- Key 값을 해시해 Partition 결정
- 동일한 Key는 항상 동일 Partition으로 매핑된다

Partition 수가 변경되면  
같은 Key라도 다른 Partition에 저장될 수 있다.

---

### Key가 null인 경우

- 사용 가능한 Partition 중 하나를 선택
- Round-robin 기반
- Sticky Partitioning 적용

Producer는 하나의 배치를 먼저 채운 뒤  
다음 Partition으로 이동한다.
