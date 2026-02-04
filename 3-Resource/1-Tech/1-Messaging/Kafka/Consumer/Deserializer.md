
컨슈머는 브로커로부터 받은 바이트 배열을
Deserializer를 통해 객체로 변환한다.

---

## 기본 설정

- key.deserializer
- value.deserializer

프로듀서의 Serializer와 반드시 호환되어야 한다.

---

## 주요 포맷

- Primitive / String
- JSON
- Avro
- Protobuf

---

## Avro 사용 이유

- 명시적 스키마
- 스키마 진화 지원
- Producer-Consumer 계약 명확화

일반적으로 Schema Registry와 함께 사용된다.
