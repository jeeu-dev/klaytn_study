# klaytn_study

#### 자료 : [inflearn - Klaytn 블록체인 어플리케이션 만들기 - 이론과 실습](https://www.inflearn.com/course/%ED%81%B4%EB%A0%88%EC%9D%B4%ED%8A%BC#)

----------

> 2019-05-08

### 1. 인트로

### 2. 기존 블록체인 플랫폼의 약점

- 기존 블록체인 플랫폼의 약점
  - **Scalability**
    - TPS + Block Interval
    - Transaction Per Second : 초당 몇개의 거래 처리
      - Visa : TPS 1700
      - 비트코인 : TPS 7
      - 이더리움 : TPS 15-20
    - Block interval : 블록 생성 간격
      - 비트코인 : 10분
      - 이더리움 : 15초-20초
  - 기존의 블록체인은 왜 느린가?
    - 기존은 노드가 많다고 속도가 빠르지 않다 - 분산하여 처리하지 않는다
  - 비트코인 & 이더리움
    - 많은 양의 트랜잭션 처리 힘듦
    - 네트워크 자체 속도 느림
- **Finality**
  - 변경 불가능한 최종 상태를 말함
  - 블록이 Final 하다는 것은 블록에 담긴 Tx(거래)가 바뀔 수 없다는 걸 보증
  - 비트코인 & 이더리움
    - 최종성 부족
    - 확률록적 최종성만 제공 - 결제는 했지만 결제가 되지 않을 수 있다는 것
    - 비트코인 - Finality까지의 평균 시간 60분(6번의 검증)
    - 이더리움 - Finality까지의 평균 시간 6분(25분)
    - Finality : TX가 변경 불가라는 합리적인 보장받기까지 기다려야되는 시간
- Fork
  - 블록들의 연결이 두개 이상의 분기로 갈라지는 현상을 말함
  - 이유 - 블록체인 p2p 사용자들이 독립적으로 채굴할 수 있기 때문

### 3. 클레이튼 이해하기

- 합의(Consensus)
  - Public : PoW, PoS, etc
  - Private : pBFT, Raft, etc
  - 클레이튼 합의 알고리즘 IBFT(이스탄불 비잔티움 결함허용)
    - 공개를 통한 개인적인 합의 신뢰 모델 채택(Private consensus with public disclosure)
      - 합의 달성 소수 Private 노드
      - 블록 생성 결과 접근 및 검증 노드
      - 라운드 로빈 방식(?)
- 블록 생성 및 전파
  - 블록 생성 사이클(cycle)
    - 블록 생성 주기 = 라운드 (round)
    - 블록 생성 간격 = 약 1초
  - 제안자와 위원회 선택 (Proposer and Committee Selection)
    - 제안자를 무작위(randomly) & 결정적 (deterministically)으로 Governance Council(합의 노드) 노드들 중 뽑음
    - 각각의 합의 노드가 가장 최근의 블록 헤더에서 파생된 난수 사용 자기가 라운드에 선택됐는지 증명
  - 블록 제안과 검증(Block Proposal and Validation)
    - 제안자의 공개키 통해 입증가능한 암호증명 사용
    - 누가 제안자고 누가 위원회인지 파악이 되면 제안자가 블록 만들고 합의
  - 블록 전파 (Block Propagation)
    - 2/3 서명을 받아야 함
    - 프록시 노드 통해 엔드포인트 노드들에게 전달 됨
- 네트워크 구조
  - 코어 셀 네트워크가 존재하고 그 코어 셀 네트워크를 둘러쌓는 엔드포인트 노드 네트워크가 존재
    - CNN : Consensus Node Network
    - PNN : Proxy Node Network
    - ENN : Endpoint Node Network
    - PN Bootnode, EN Bootnode
    - 각 역할이 다름
- 코어 셀(Core Cell)
  - 메인넷이 런칭되면 몇 십대가 될 듯
  - 사용자가 많아져서 확장이 필요할 때
    - 일반적 : 서버 늘리고 Request 분할 처리
    - 클레이튼 : 노드 자체의 성능을 늘림 - Ex 램이나 cpu 성능 늘림
  - CN(합의 노드) 참여 조건
    - Physical core가 40개 이상
    - 256GB RAM
    - 1년치 데이터 약 14TB 저장
    - 10G 네트워크
- 서비스 체인
  - 언제 쓰이나?
    - 특별한 노드환경에서 설정
    - 보안 수준 맞춤형으로 설정
    - 많은 처리량 요구 / 메인넷 배포시 경제성 낮음
  - 독립된 서비스 공간을 구축해서 필요할 때 메인넷에 신뢰를 고정
  - Gas 비용 안받게 설정 가능
- 이더리움과 클레이튼의 차이
  - 이더리움
    - 단일 네트워크
    - 가장 먼저 블록을 만들고 많이 전파해야 함
    - PoW
    - 마이닝 노드 : 블록을 쓰고 네트워크에 전파한 노드
  - 클레이튼
    - Two Layer Architecture Trust Model
    - 매 라운드마다 합의노드들 중 하나가 뽑혀서 블록을 씀

### 4. 클레이튼 덧셈게임 개발 with Klaytn Tools

- 인트로
  - 이더리움의 비잔티움 버전 Fork
  - 클레이튼 caver.js
  - 이더리움 web3.js
  - 솔리딭 Solidity
  - 트러플(Truffle) 프레임워크
  - IDE
  - Wallet
  - Scope
- Klaytn Wallet & 계정관리
  - [https://baobab.klaytnwallet.com](https://baobab.klaytnwallet.com/)
- Klaytn IDE & 스마트 계약1
  - http://ide.klaytn.com/
- 