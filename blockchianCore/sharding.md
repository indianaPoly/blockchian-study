# 블록체인 샤딩

**샤딩(sharding)** 이란 블록체인의 확장성을 해결하기 위한 기술

- 네트워크 처리량을 증가시키고 속도를 개선
- 전체 블록체인을 여러 작은 _shard_ 로 나누는 방식
- 각 샤드는 독립적으로 tx를 처리, 블록체인의 모든 데이터를 저장하지 않음.

**목표** : 모든 노드가 전체 블록체인을 관리하는 것이 아니라, 각 노드가 특정한 shard만 관리하여 처리 성능을 높임

## 구성 요소

### 기본 구조 정의

```go
package main

import (
  "crypto/sha256"
  "encoding/hex"
  "fmt"
)

type Tx struct {
  Sender  string
  Reciver string
  Amount  int
}

type Shard struct {
  ID    int
  Txs   []Tx
  State map[string]int
}

func NewShard(_ID int) *Shard {
  return &Shard{
    ID:       _ID,
    Txs:      []Tx{},
    State:    make(map[string]int),
  }
}

func (s *Shard) AddTx(_Tx Tx) {
  s.Txs = append(s.Txs, _Tx)
}

func (s *Shard) UpdateState(sender, receiver string, amount int) {
  if s.State[sender] >= amount {
    s.State[sender] -= amount
    s.State[receiver] += amount
  }
}

func (s *Shard) ProcessTxs() {
  for _, tx := range s.Txs {
    s.UpdateState(tx.Sender, tx.Receiver, tx.Amount)
  }

  // 트랜젝션 처리 이후에 초기기화 진행
  s.Txs = []Tx{}
}

func (s *Shard) PrintState() {
  fmt.Printf("Shard %d state: %v\n", s.ID, s.State)
}

```

### 주요 구성 요소

- 네트워크 샤딩
  - 네트워크 자체를 분할해 각 샤드에서 특정 노드가 통신하며, 네트워크 부담을 분산
  - 네트워크를 여러 샤드로 나누고, 트랜젝션을 샤드에 할당하는 방식 (정보를 해시로 변환하여 샤드에 분배)

```go
// 해당 코드는 트랜젝션의 고유한 아이디에 따라서 특정 샤드에 할당하는 방식으로 구현
type Network struct {
  Shards []*Shard
}

func NewNetwork(numShards int) *Network {
  shards := make([]*Shard, numShards)
  for i := 0; i < numShards; i++ {
    shards[i] = NewShard(i)
  }
  return &Network{Shards: shards}
}

func (n *Network) GetShardForTx(tx Tx) *Shard {
  hash := sha256.New()
  hash.Write([]byte(tx.Sender + tx.Reciver))
  hashValue := hex.EncodeToString(hash.Sum(nil))
  shardID := int(hashValue[0]) % len(n.Shards)
  return n.Shards[shardID]
}

func (n *Network) AddTxToShard(tx Tx) {
  shard := n.GetShardForTx(tx)
  shard.AddTx(tx)
}

func (n *Network) ProcessAllShards() {
  for _, shard := range n.Shards {
    shard.ProcessTxs()
    shard.PrintState()
  }
}
```

- 거래 샤딩 : 트랜잭션 샤드로 분리하여 갹 샤드가 독립적으로 트랜젝션을 처리

```go
func (n *Network) GetShardForTxByAmount(tx Tx) *Shard {
  // 트랜잭션의 금액을 기준으로 샤드 결정
  shardID := tx.Amount % len(n.Shards)
  return n.Shards[shardID]
}

func (n *Network) AddTxToShardByAmount(tx Tx) {
  shard := n.GetShardForTxByAmount(tx)
  shard.AddTx(tx)
}
```

- 상태 샤딩 : 블록체인의 상태 정보를 분할하여 각 샤드가 특정한 상태 데이터만 관리

```go
func (n *Network) GetShardForState(account string) *Shard {
  hash := sha256.New()
  hash.Write([]byte(account))
  hashValue := hex.EncodeToString(hash.Sum(nil))
  shardID := int(hashValue[0]) % len(n.Shards)
  return n.Shards[shardID]
}

func (n *Network) UpdateStateInShard(sender, receiver string, amount int) {
  senderShard := n.GetShardForState(sender)
  receiverShard := n.GetShardForState(receiver)

  if senderShard != receiverShard {
    // 크로스 샤드 처리: 송신자 샤드에서 금액 감소, 수신자 샤드에서 금액 증가
    if senderShard.State[sender] >= amount {
      senderShard.State[sender] -= amount
      receiverShard.State[receiver] += amount
    } else {
      fmt.Println("잔액 부족:", sender)
    }
  } else {
    // 같은 샤드 내 처리
    senderShard.UpdateState(sender, receiver, amount)
  }
}

func (n *Network) PrintAllShardStates() {
  for _, shard := range n.Shards {
    shard.PrintState()
  }
}
```
