# Transaction \(트랜잭션\)

트랜잭션을 통해 사용자\(users\)는 사토시를 소비할 수 있다. 각 트랜잭션은 간단하고 직접적인 거래\(simple direct payments\)부터 복잡한 거래를 가능케 하는 몇개의 부분\(several parts\)으로 구성되어 있다. 이 섹션에서는 트랜잭션을 이루고 있는 각 부분에 대한 설명과 그 부분들을 이용하여 완전한 트랜잭션을 구성하는 방법을 다룬다.

설명을 간단히 하기위해, 이 섹션에서는 [코인베이스 트랜잭션](https://bitcoin.org/en/glossary/coinbase-transaction)\(coinbase transactions\)은 존재하지 않는다고 가정한다. 코인베이스 트랜잭션은 비트코인 마이너만이 만들 수 있는 특별한 트랜잭션이며 코인베이스 트랜잭션은 아래에서 설명될 많은 규칙\(제약\)들의 영향을 받지 않는다. 각 규칙에 영향을 받지 않는 [코인베이스](https://bitcoin.org/en/glossary/coinbase-transaction)\(coinbase\)에 대해 자세히 알고 싶다면, [블록체인 섹션](https://books-and-cows.gitbooks.io/bitcoin-developer-guide-in-korean/content/blockchain.html)의 코인베이스 트랜잭션파트를 참고하길 바란다; [블록체인 섹션](https://books-and-cows.gitbooks.io/bitcoin-developer-guide-in-korean/content/blockchain.html)에서 '코인베이스' 키워드로 검색을 해서 해당 내용을 살펴보면 된다.

\(그림\)



