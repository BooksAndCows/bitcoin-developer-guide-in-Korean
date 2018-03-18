# Transaction \(트랜잭션\)

트랜잭션을 통해 사용자\(users\)는 사토시를 소비할 수 있다. 각 트랜잭션은 간단하고 직접적인 거래\(simple direct payments\)부터 복잡한 거래를 가능케 하는 몇개의 부분\(several parts\)으로 구성되어 있다. 이 섹션에서는 트랜잭션을 이루고 있는 각 부분에 대한 설명과 그 부분들을 이용하여 완전한 트랜잭션을 구성하는 방법을 다룬다.

설명을 간단히 하기위해, 이 섹션에서는 [코인베이스 트랜잭션](https://bitcoin.org/en/glossary/coinbase-transaction)\(coinbase transactions\)은 존재하지 않는다고 가정한다. 코인베이스 트랜잭션은 비트코인 마이너만이 만들 수 있는 특별한 트랜잭션이며 코인베이스 트랜잭션은 아래에서 설명될 많은 규칙\(제약\)들의 영향을 받지 않는다. 각 규칙에 영향을 받지 않는 [코인베이스](https://bitcoin.org/en/glossary/coinbase-transaction)\(coinbase\)에 대해 자세히 알고 싶다면, [블록체인 섹션](https://books-and-cows.gitbooks.io/bitcoin-developer-guide-in-korean/content/blockchain.html)의 코인베이스 트랜잭션파트를 참고하길 바란다; [블록체인 섹션](https://books-and-cows.gitbooks.io/bitcoin-developer-guide-in-korean/content/blockchain.html)에서 '코인베이스' 키워드로 검색을 해서 해당 내용을 살펴보면 된다.

\(그림\)

각 트랜잭션 앞에는 4바이트의 트랜잭션 버전 번호\(transaction version number\)가 들어 있다; 트랜잭션 버전 번호는 비트코인 네트워크를 구성하는 피어들과 마이너들이 트랜잭션을 검증할 때 어떤 규칙들을 적용하는지를 알려준다. 버전 번호가 있음으로써 개발자들은 새로운 규칙을 개발할 때, 이전 규칙을 적용하는 트랜잭션들을 무효로 처리하지 않으면서 개발할 수 있다; 즉, 하위호환이 가능하다.

\(그림\)

[아웃풋](https://bitcoin.org/en/glossary/output)\(output\)은 트랜잭션 내의 위치를 나타내는 인덱스 번호를 갖는다 - 첫 번째 아웃풋의 인덱는 0이다. 또한 아웃풋은 조건부 [pubkey script](https://bitcoin.org/en/glossary/pubkey-script)를 소비하는 사토시의 양을 가지고 있다. pubkey script에 명시된 조건을 만족시킬 수 있다 해당 pubkey script를 소비하는 사토시를 소비할 수 있다.

> 트랜잭션에 존재하는 아웃풋\(output\)은 2가지 필드를 포함한다: a value field: 전송하려는 0이상의 사토시가 기록되는 필드, pubkey script: value field에 기록된 사토시를 소비하기 위해 반드시 만족시켜야하는 조건들을 명시해 놓은 필드.

인풋\(input\)은 사용될 특정 아웃풋을 식별하기 위해 트랜잭션 식별자\(txid\)와 아웃풋 인덱스 번호\(output vector의 "vout"이라 불린다\)를 사용한다. 또한 인풋은 [signature script](https://bitcoin.org/en/glossary/signature-script)를 가지는데, 이 signature script는 인풋이 pubkey script에 존재하는 조건들을 만족시키는 매개변수들을 생성할 수 있게 해준다. \(The [sequence number](https://bitcoin.org/en/glossary/sequence-number)와  [locktime](https://bitcoin.org/en/glossary/locktime) 은 서로 관련이 있으며 뒤에 나오는 서브섹션에서 다뤄질 것이다.\)

아래의 그림은 위에서 설명한 것들이 어떻게 사용되는지를 나타내고 있다. Alice가 Bob에게 트랜잭션을 보냈고, 나중에 Bob이 해당 트랜잭션을 사용하는 과정\(workflow\)이다. Alice와 Bob 둘 다 가장 보편적인 형식인 Pay-To-Public\_Key-Hash\(P2PKH\) 트랜잭션 타입을 사용할 것이다. P2PKH는 Alice가 비트코인 [주소](https://bitcoin.org/en/glossary/address)\(address\)로 사토시를 전송할 수 있게 해주며, 그 후 Bob이 간단한 암호학 방법인 key pair를 사용하여 해당 사토시를 소비할 수 있게 해준다; Alice가 보낸 사토시를 Bob에게 보냈으니 Bob이 소비할 수 있다는 건 그다지 어려운 개념이 아니다.

\(그림\)

첫번째로 Bob은 Alice가 트랜잭션을 생성하기 전에 반드시 개인키/공개키 쌍\(private/public key pair\)를 만들어야 한다. 비트코인 시스템은 [secp256k1](http://www.secg.org/sec2-v2.pdf) 곡선과 함께 타원곡선암호화 서명 알고리즘\([ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)\)을 사용한다; secp256k1 개인키는 256비트로 구성된 임의 데이터\(random data\)다. 해당 개인키의 복사본은 결정론적으로 secp256k1 공개키로 변환된다; 결정론적이라는 건, 하나의 개인키에 대응되는 전세계 단 하나뿐인 공개키를 생성한다는 뜻이다. 개인키를 공개키로 변환하는 과정은 계속 반복될 수 있기 때문에, [공개키](https://bitcoin.org/en/glossary/public-key)\(public key\)를 따로 저장할 필요는 없다.

공개키\(pubkey\)는 암호학적으로 해시된다. 공개키가 해시된 값인 [pubkey hash](https://bitcoin.org/en/glossary/p2pkh-address)는 후에 또 반복적으로 생성될 수 있으므로, 따로 공개키와 마찬가지로 따로 저장할 필요는 없다; 비트코인의 주소는 공개키가 해시된 값, pubkey hash를 포함하고 있으며, 이 pubkey hash는 비트코인을 소비하는 사용자가 표준 [pubkey script](https://bitcoin.org/en/glossary/pubkey-script)\([P2PKH](https://bitcoin.org/en/glossary/p2pkh-address)\)를 생성할 수 있게 해준다. 해시는 공개키를 단축하고 난독화하여 수동으로 쉽게 기록할 수 있게하고 예기치 않은 문제에 대한 보안을 제공하므로 나중에 공개키 데이터로부터 개인키를 재구성하는 걸 가능케 해준다.

Bob은 pubkey hash를 Alice에게 제공한다. Pubkey hashes는 항상 비트코인 주소로 암호화\(encoded\)되어 전송된다; base58 방식으로 암호화된 문자열은 [지불 주소](https://bitcoin.org/en/glossary/address)\(address\), 해시, 오타 검사용 체크섬을 포함한다. 비트코인 주소는 

