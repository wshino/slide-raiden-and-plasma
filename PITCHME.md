### raiden and plasma

---

### 私
*所属*

- ビッグデータ部
- レコメンドチーム

---

### 私
*やってること*

- DevOps
  - Kubernetes, Spinnaker, Istio
- API
  - Akka HTTP
- Recommend Batch
  - Spark
- SQL はちょと苦手

---

### 私
*Next Currency*

- Wallet周りのアーキテクチャ設計

---

### ゴール

- ブロックチェーンのスケーラビリティ問題を理解する
- Raiden, Plasmaについて理解する

---

### スケーラビリティ問題

- 全てのノードが同じタスクを処理している
- 全てのノードが同一の状態を持つ
- ノード数を2倍にしても各ノードのタスク量は同じ
- ノードを増やしてもスケールしない

---

### スケーラビリティへの解決方法

- オフチェーン
- オンチェーン

---

### スケーラビリティへの解決方法 
*オフチェーン*

- タスクの一部をブロックチェーンの外側で処理
- ノード全体でデータを共有する必要がない
- 実装例
  - Raiden
  - TrueBit
  - Lightning Network

---

### スケーラビリティへの解決方法 
*オフチェーン*

- ブロックチェーンで処理すべきタスク量を減らしているだけ
- スケールしない問題を解決していない
- パフォーマンスの問題は解決できる

<!--- 
なお、少しややこしいですが、Lightning NetworkやRaidenは、ビットコインやイーサリアムとは独立したプラットフォームであり、それら自体はスケーラビリティのあるアーキテクチャとなっています。
-->

---

### スケーラビリティへの解決方法 
*オンチェーン*

- タスクやデータを分割して、分割されたタスクやデータを並列で処理
- MapReduce
- 実装例
  - シャーディング
  - Plasma

---

### 前提知識

- マルチシグアドレス
- ペイメントチャネル
- 単方向ペイメントチャネル
- 双方向ペイメントチャネル
- Ethereumはトランザクションでステートがブロックごとに変化していく

---

### Raiden

- Lightning Network <- 前提知識に移動？
- Hashed Timelock Contract <- 前提知識に移動？
- オフチェーンでトランザクションを処理する
- オフチェーンではERC20準拠トークンの受け渡ししかできない

---

### Raiden
*Lightning Networkとの違い*

- Lightning NetworkはBTCのみ
- RaidenはERC20準拠トークン
- ETHはWETHにする
- WETH = Wrapped ETH

---

### Raiden
*メリット*

- 手数料かからない
- オープンチャネルとクローズのコミット以外
- オフチェーンの取引は公開されない
- TK 視聴課金などの具体例

---

### Raiden
*デメリット*

- あらかじめデポジットした量を超えてやりとり不能

---

### Raiden
*Raidenの種類*

- μRaiden
- raiden network
- raidos

---

### Raiden
*μRaiden*

- 片方向のチャネル
- dapps対ユーザーなどの1対nのユースケース
- ERC20とERC223に準拠した専用のRDNトークン

---

<!---

### Raiden
*μRaiden*
#### デモ

- オンチェーンにpayment channelを作成するのは15秒かかる
- 開いたらあとは爆速
- 0.5秒でトランザクション完了
- 足りなくなったらオンチェーンに書き込む、時間がかかる
- クローズも時間かかる15秒かかる
- オフチェーンの取引はgasがかからない
- ERC20とERC223に準拠した専用のRDNトークン

---
--->

### Raiden
*Raiden Network*

- μRaidenを相互接続する？
- n対nの双方向
- バケツリレーでトークン転送できる
- チャネル経由の際にインセンティブを経由に与えるのでコストはかかる

---

### Raiden
*Raidos*

- スマートコントラクトの実行をサイドチェーンで行う
- 構想段階でよくわからん

---

### Raiden
*ICO*

- Raidenで使うRDNトークンの売り出し
- 類似プラットフォームの乱立を回避
- フルノードの維持費
- サードパーティツールのインセンティブ

---

### Raiden
*プロジェクト*

- raidEX
  - 高速なDEXを実現する
- Trustlines Network
  - Ripple on Ethereal

---

### Raiden
*課題*

- 中継経路の効率的探索

---

### Plasma
*概要*

- main chain にサイドチェーンを作成(child chain)
- child chainで処理する
- 必要に応じてメインチェーンに書き込む
- child chain はメインチェーンと構造が同じ
- メインチェーン全部を見なくても、child chain だと必要なものしか見なくて良い

---

### Plasma
*処理の詳細*

- main chain と child chain は階層構造
- main chainでやるタスクを細分化して
- child chainに分配
- 処理が終わった結果をreduceする
- main chainに戻す

---

### Plasma
*処理性能の向上例*

- main chainが10tx/sec
- 3分割したchild chainで30tx/sec
- さらに3分割したら90tx/sec
- さらに3分割したら270tx/sec

---

### Plasma
*特徴*

- スマートコントラクトも動作可能
- Raidenのraidosに似ている
- 不正防止
- fraud proof

---

### 両者の違い

- raiden
  - トランザクションの高速化
  - トークンの受け渡しを速くする
- plasma
  - スループットの向上
  - MapReduce

<!--- 大量の処理を高速化するのには向いているけど、各々のトランザクションを早くするのには向かないかも？ --->  