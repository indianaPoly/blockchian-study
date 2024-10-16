# Node Architecture: Ethereum 노드 아키텍처

## 1. 구성 요소: 실행 클라이언트와 합의 클라이언트

### Geth (실행 클라이언트)

- 트랜잭션 처리와 상태 업데이트 (EVM에서 수행)
- 블록 생성 및 네트워크에 전파
- RPC 메소드를 통해 쿼리, 트랜잭션 제출, 스마트 컨트랙트 배포 지원

### 합의 클라이언트

- 블록 생성 순서 결정
- 합의 알고리즘 처리 및 검증 조정

---

## 2. 전체 동작 흐름

1. **합의 클라이언트**가 Geth에 트랜잭션 번들을 전달 → 규칙 준수 여부 확인
2. **Geth**가 트랜잭션을 하나씩 실행 → 유효성 검증
3. **Geth**가 블록 생성 차례가 되면, 합의 클라이언트가 요청
4. **Geth**가 새 블록을 생성하고 네트워크에 전파

---

## 3. 통신 방식

- **RPC와 엔진 API**: Geth와 합의 클라이언트 간 통신 처리
- **P2P 네트워크**: 노드 간 트랜잭션과 블록 동기화

---

## 4. 코드 예시

### 트랜잭션 실행 예시

```go
func (p *StateProcessor) Process(block *types.Block, statedb *state.StateDB) (types.Receipts, error) {
    var receipts types.Receipts // 트랜젝션의 영수증을 저장하는 배열

    for i, tx := range block.Transactions() { // 블록 안에 있는 모든 트랜젝션을 순회하면서
        receipt, err := ApplyTransaction(statedb, tx) // 트랜잭션을 실행함.
        if err != nil { // 에러가 발생하면
            return nil, err // 에러를 반환하고
        }
        receipts = append(receipts, receipt) // 영수증을 배열에 추가함
    }
    return receipts, nil // 모든 트랜젝션이 성공적으로 처리하면 배열을 반환합니다.
}
```
