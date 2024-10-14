# **go-ethereum 분석을 통한 블록체인 코어 학습: 체크리스트**

각 항목을 하루에 하나씩 완료하고 체크(✔️)하세요.

---

## **Chapter1: 노드 초기화 및 실행 (`cmd/geth`)**

- [ ] **Day 1:** [`cmd/geth/main.go`에서 `main` 함수의 역할과 초기화 순서 분석](../notes/chapter1/day1-main-function.md)
- [ ] **Day 2:** `cli.Context` 활용한 플래그 파싱 분석 (`cmd/geth/main.go`)
- [ ] **Day 3:** 주요 함수 호출 흐름과 `makeFullNode` 호출 과정 이해
- [ ] **Day 4:** `internal/flags/flags.go`에서 주요 플래그 분석 (`--syncmode`, `--datadir` 등)
- [ ] **Day 5:** `node.New`와 노드 구성 (`node/node.go`)
- [ ] **Day 6:** 데이터 디렉터리 초기화 (`chaindata`)와 키스토어 로딩
- [ ] **Day 7:** 노드 시작과 종료 (`node/service.go`) 이해 및 인터럽트 처리

---

## **Chapter2: 블록체인 데이터 구조 (`core/`)**

- [ ] **Day 8:** `core/types/block.go`에서 `Block` 구조체 분석
- [ ] **Day 9:** 블록 해시와 넘버 매핑 (`block_hash.go`)
- [ ] **Day 10:** `Transaction` 구조체와 RLP 인코딩 분석 (`core/types/transaction.go`)
- [ ] **Day 11:** 트랜잭션 서명과 유효성 검증
- [ ] **Day 12:** `StateDB`와 상태 저장 구조 (`core/state/statedb.go`)
- [ ] **Day 13:** 머클 패트리샤 트리 분석 (`core/state/`)
- [ ] **Day 14:** 스테이트 트리의 커밋과 롤백

---

## **Chapter3: 블록 생성 및 검증 (`core/`)**

- [ ] **Day 15:** `core/block_validator.go`에서 블록 유효성 검증 로직 이해
- [ ] **Day 16:** 블록 헤더 및 타임스탬프 검증
- [ ] **Day 17:** 트랜잭션 가스 검증 분석
- [ ] **Day 18:** 블록 해시 유효성 확인
- [ ] **Day 19:** `core/state_processor.go`에서 트랜잭션 실행 흐름 이해
- [ ] **Day 20:** 가스 소모 추적과 EVM 실행 분석
- [ ] **Day 21:** 실패한 트랜잭션 롤백 처리

---

## **Chapter4: 트랜잭션 풀 (`eth/txpool/`)**

- [ ] **Day 22:** `eth/txpool/txpool.go`에서 트랜잭션 풀 초기화
- [ ] **Day 23:** 트랜잭션 유효성 검증 및 추가 로직
- [ ] **Day 24:** 트랜잭션 브로드캐스트 처리 분석
- [ ] **Day 25:** 가스 가격 기반 우선순위 설정 (`eth/txpool/priorities.go`)
- [ ] **Day 26:** 풀 큐와 대기열 정리
- [ ] **Day 27:** 만료된 트랜잭션 삭제 조건
- [ ] **Day 28:** EVM과 트랜잭션 풀 상호작용 이해 (`eth/txpool/executor.go`)

---

## **Chapter5: EVM 분석 (`core/vm/`)**

- [ ] **Day 29:** EVM 인스턴스 초기화 (`core/vm/evm.go`)
- [ ] **Day 30:** 바이트코드 명령어 실행과 스택 조작
- [ ] **Day 31:** `core/vm/gas_table.go`에서 가스 소비 계산
- [ ] **Day 32:** 동적 가스 계산 로직 분석 (`CALL`, `CREATE`)
- [ ] **Day 33:** 메모리 할당과 확장 로직
- [ ] **Day 34:** 스택 언더플로우와 오버플로우 처리
- [ ] **Day 35:** 스마트 컨트랙트 배포 과정 이해

---

## **Chapter6: 합의 알고리즘 (`consensus/`)**

- [ ] **Day 36:** PoW 초기화 및 난이도 조정 로직 (`consensus/ethash/ethash.go`)
- [ ] **Day 37:** 블록 해시 계산 분석
- [ ] **Day 38:** 마이닝과 Nonce 발견 과정
- [ ] **Day 39:** PoS와 PoW의 차이점 이해 (`consensus/clique/clique.go`)
- [ ] **Day 40:** 검증자 선출과 블록 생성 로직 분석
- [ ] **Day 41:** 합의 알고리즘 인터페이스 정의 (`consensus/consensus.go`)
- [ ] **Day 42:** PoA와 PoS 전환 실험 준비

---

## **Chapter7: PoA 및 PoS 실험 (`consensus/`)**

- [ ] **Day 43:** PoA 설정 및 테스트 (`consensus/clique/clique.go`)
- [ ] **Day 44:** PoA에서 피어 네트워크 설정 실험
- [ ] **Day 45:** PoW에서 PoS로의 전환 테스트
- [ ] **Day 46:** PoS 블록 검증과 슬래싱 처리 실험
- [ ] **Day 47:** 합의 알고리즘 변경 중 발생 가능한 오류 처리
- [ ] **Day 48:** PoS에서 트랜잭션 처리 성능 분석
- [ ] **Day 49:** PoA 및 PoS 성능 비교
