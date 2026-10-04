# smart-koutei 書籍ガイド

このドキュメントは、smart-koutei を進める中で、

> 今の設計論点に対して、どの本を読むべきか

を判断するために使う。

最初から通読しない。  
困った論点に対応する本を、その都度読む。

---

## 1. 要求・ユースケース

### ユーザーストーリーマッピング

**読む目的**

- ユーザー業務全体を整理する
- Epic / Story を整理する
- 今回どこまで作るかを決める

**読むとき**

- Scope が広すぎる
- 機能一覧になっている
- Vertical Slice を決めたい

---

### ユースケース実践ガイド

**読む目的**

- Actor / Goal を整理する
- Main Flow / Alternative Flow を考える
- 正常系・例外系を明確にする

**読むとき**

- Use Case を具体化するとき
- CRUD ではなく業務上の振る舞いを考えたいとき
- Domain Rule を発見する材料が欲しいとき

---

# 2. DDD / Domain Modeling

### ドメイン駆動設計をはじめよう

**DDD のメイン教材。**

**読む目的**

- Entity / Value Object
- Invariant
- Aggregate
- Domain Service
- Bounded Context
- Transaction Boundary

を理解する。

**読むとき**

- Domain Rule から Domain Model へ進む
- Entity / Value Object で迷う
- Aggregate Boundary を考える
- Transaction Boundary を考える

**特に重要**

- Aggregate は何をまとめるものか
- Invariant をどこで守るか
- Aggregate と Transaction Boundary の関係

---

### 実践ドメイン駆動設計

**DDD を深掘りするときに使う。**

**読む目的**

- Aggregate
- Repository
- Application Service
- Domain Service
- Domain Event

を詳しく理解する。

**読むとき**

- 『ドメイン駆動設計をはじめよう』だけで判断できない
- Aggregate や Repository を深く考えたい
- 複数 Aggregate 間の処理を考えたい

**注意**

必要になる前にパターンを導入しない。

---

### ドメイン駆動設計入門

**DDD の簡易リファレンス。**

**読む目的**

- Entity
- Value Object
- Repository
- Domain Service

などを短時間で復習する。

**読むとき**

- DDD 用語を確認したい
- 他の DDD 本が重いとき

---

### アナリシスパターン

**業務モデルを深く考えるための本。**

**読む目的**

- 現実世界の業務概念をどうモデル化するか考える
- 時間、関係、数量などのモデルを考える

**読むとき**

- 一度 Domain Model を作った後
- モデルに違和感がある
- 業務概念をうまく表現できない

**注意**

最初からパターンを当てはめない。

---

# 3. Data Modeling / Database

### データモデリングでドメインを駆動する

**読む目的**

- Domain Model と Data Model の違いを理解する
- 業務概念を RDB へ落とす

**読むとき**

- Domain Model から DB 設計へ進む
- Entity 間の Relation を考える
- Domain Model と Table をそのまま 1:1 にしそうなとき

---

### 達人に学ぶ DB 設計 徹底指南書

**RDB 設計の基本教材。**

**読む目的**

- 正規化
- PK / FK
- UNIQUE
- NOT NULL
- CHECK
- Index

を理解する。

**読むとき**

- PostgreSQL Schema を設計する
- DB でどこまで整合性を守るか考える
- Relation や Constraint で迷う

---

### SQL アンチパターン

**DB 設計レビュー用。**

**読む目的**

- 壊れやすい Schema を知る
- よくある DB 設計ミスを避ける

**読むとき**

- Prisma Schema を一度作った後
- JSON / NULL / type 列が増えてきた
- Relation が不自然になってきた

---

# 4. Transaction / Concurrency

### データ指向アプリケーションデザイン

**Concurrency を考えるときのメイン教材。**

**読む目的**

- Transaction
- Isolation Level
- Lost Update
- Write Skew
- Serializable
- Consistency

を理解する。

**読むとき**

- 同じ設備へ同時に予定を入れられる可能性がある
- 同じ受注を同時編集する
- Capacity 確認後、保存までに状態が変わる
- Optimistic / Pessimistic Lock を考える

smart-koutei では特に重要。

---

### 詳説 データベース

**DB 内部を深く理解するときの本。**

**読む目的**

- B-Tree
- WAL
- Page
- Storage Engine
- Database Internals

を理解する。

**読むとき**

- Index や Transaction を内部構造から理解したい
- DB の性能まで深掘りしたい

**優先度**

後半でよい。

---

# 5. Software Architecture

### ソフトウェアアーキテクチャの基礎

**Architecture のメイン教材。**

**読む目的**

- Modularity
- Coupling / Cohesion
- Architecture Characteristics
- Architecture Style
- Trade-off

を理解する。

**読むとき**

- モジュール境界を考える
- Application / Domain / Infrastructure を整理する
- Modular Monolith を考える
- Architecture Decision を説明したい

---

### Clean Architecture

**依存関係と責務を考える本。**

**読む目的**

- Dependency Rule
- Use Case Boundary
- Interface Adapter
- Domain を Framework から独立させる

**読むとき**

- Hono や Prisma が Domain に入り込んできた
- Application Layer を整理したい
- Repository Interface が必要か迷う

**注意**

Clean Architecture の図を再現することを目的にしない。

必要な分だけ使う。

---

### エンタープライズアプリケーションアーキテクチャパターン

**Application Architecture を深掘りするときの本。**

**読む目的**

- Transaction Script
- Domain Model
- Repository
- Unit of Work
- Data Mapper
- Active Record

などを理解する。

**読むとき**

- Domain Model を使うべきか迷う
- Repository や ORM との境界を考える
- Application Architecture の選択肢を増やしたい

---

# 6. Code Design

### Good Code, Bad Code

**読む目的**

- Encapsulation
- Abstraction
- Dependency
- Testability
- Error Handling

を改善する。

**読むとき**

- Domain Model を TypeScript で実装する
- Class / Function が大きくなった
- Domain Rule がいろいろな場所へ漏れた
- コードレビューするとき

---

# 7. Test

### TDD

**読む目的**

- Test First
- Feedback Loop
- Refactoring
- Design Discovery

を理解する。

**読むとき**

- Domain Rule を Test にする
- Invariant をどうテストするか考える
- Domain Model の API が使いづらい
- Refactoring する

---

# 8. 迷ったときの対応表

| 今困っていること                | 読む本                                                            |
| ------------------------------- | ----------------------------------------------------------------- |
| Scope が広すぎる                | ユーザーストーリーマッピング                                      |
| Use Case が曖昧                 | ユースケース実践ガイド                                            |
| Entity / Value Object           | ドメイン駆動設計をはじめよう                                      |
| Invariant                       | ドメイン駆動設計をはじめよう                                      |
| Aggregate                       | ドメイン駆動設計をはじめよう / 実践 DDD                           |
| Domain Model に違和感           | アナリシスパターン                                                |
| Domain → DB                     | データモデリングでドメインを駆動する                              |
| 正規化 / FK / UNIQUE            | 達人に学ぶ DB 設計                                                |
| DB 設計が怪しい                 | SQL アンチパターン                                                |
| 同時更新                        | データ指向アプリケーションデザイン                                |
| Transaction Boundary            | ドメイン駆動設計をはじめよう / データ指向アプリケーションデザイン |
| モジュール分割                  | ソフトウェアアーキテクチャの基礎                                  |
| Domain と Infrastructure の依存 | Clean Architecture                                                |
| Repository / Unit of Work       | エンタープライズアプリケーションアーキテクチャパターン            |
| 実装が汚い                      | Good Code, Bad Code , 良いコード／悪いコードで学ぶ設計入門        |
| テストから設計を改善            | TDD ,単体テストの考え方・使い方                                   |

---

# AI へのルール

設計レビュー中に関連する書籍がある場合、AI は必要に応じて以下を提示する。

> **今読むと良い:** 『書籍名』  
> **テーマ:** Aggregate / Isolation Level / 正規化 など  
> **理由:** 今検討している設計判断に直接関係するため

本を読むこと自体を目的にしない。

必要な箇所だけ読み、必ず smart-koutei の設計へ戻る。
