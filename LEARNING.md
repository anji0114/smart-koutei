# smart-koutei Design Learning Guide

このプロジェクトの第一目的は、プロダクトを完成させることではない。

実際の業務要求を起点に、Domain Rule / Invariant / Aggregate / Data Model / Transaction / Concurrency まで自分で設計し、実装によって設計を検証できるようになることを目的とする。

## Learning Policy

設計の判断は自分で行う。AI は完成した設計を先に提示する設計者としては使わず、考えを整える・書き起こす・レビューする・実装の細部を書く役として使う（詳細は `AGENTS.md` の「役割分担」）。

基本ループ:

1. 自分で要求・ルール・モデル案を作る
2. Codex にレビューさせる
3. 指摘された問題が、どの要求に由来するか確認する
4. 自分で修正案を考える
5. 実装・テストで設計仮説を検証する
6. ADR に判断と Trade-off を残す

Codex へは「最適解を作って」ではなく、原則として以下を依頼する。

- 見落としている要求を指摘して
- この Invariant は本当に業務要求から必要かレビューして
- Aggregate Boundary が大きすぎないかレビューして
- Transaction Boundary と Aggregate Boundary が矛盾していないかレビューして
- RDB 制約で守れるものと Application で守るものを分けてレビューして
- Concurrency で壊れるケースを列挙して
- 将来要件を理由に Overengineering している部分を指摘して

## Learning Scope

全 Epic を詳細化してから実装しない。

まず以下の Vertical Slice だけを設計・実装する。

> 既に工程が定義されている受注について、各工程を設備と日付に割り当て、工程順序と設備能力を満たすスケジュールを作成・変更する。

この Slice を選ぶ理由:

- 単純 CRUD で終わらない
- Domain Rule を発見できる
- Invariant を考えられる
- Aggregate Boundary を考える必要がある
- Capacity を跨ぐため Transaction / Concurrency の議論が発生する
- PostgreSQL 制約と Domain Model の役割分担を考えられる
- 後から外注・進捗・自動最適化を追加した際の変更耐性を確認できる

## Step 1: Requirement / Use Case

### 自分で考えること

まず User Story を大量に作らない。

この Vertical Slice に必要な Use Case だけを 3〜5 個程度に絞る。

例として考える観点:

- スケジュールを新規作成する
- 未計画工程を設備へ割り当てる
- 既存工程の日程を変更する
- 設備負荷を確認する
- 不可能な計画を検出する

この段階では Entity / Aggregate / Repository を考えない。

### 読む DDD テーマ

- Ubiquitous Language
- Domain / Subdomain
- Use Case と Domain Model の違い
- Domain Rule とは何か

### Exit Criteria

- 誰が、何を判断する Use Case か説明できる
- Input / Decision / Constraint / Output を説明できる
- Scope 外の要求を切れている

---

## Step 2: Domain Rule / Invariant

### 自分で考えること

各 Use Case について「何が起きたら業務上おかしいか」を列挙する。

例として検討する問い:

- 後工程を前工程より先に予定してよいか
- 1 工程を複数設備に同時割当してよいか
- 設備能力を超えた計画を保存してよいか
- 受注の納期を超える計画は保存可能か
- 予定開始日と予定完了日の関係はどうあるべきか
- スケジュール変更時、後工程をどう扱うか

重要:

「不便だから」ではなく「業務として壊れるから守る必要がある」ものを Invariant 候補とする。

### 読む DDD テーマ

- Entity
- Value Object
- Domain Service
- Invariant
- Always-valid domain model

### Exit Criteria

各ルールについて以下を説明できる。

- 何を守るルールか
- どの要求から生まれたか
- 破ると何が起きるか
- 強制する Invariant か、警告でよい Business Rule か

---

## Step 3: Domain Model

### 自分で考えること

Domain Rule を自然に表現できるモデルを考える。

候補となる概念は先に固定しないが、現在の言葉としては以下がある。

- 受注（英語名は `docs/glossary.md` で決める）
- Operation / 工程
- Equipment / 設備
- Capacity / 能力
- Schedule / 日程

Entity や Value Object にすること自体を目的にしない。

### 読む DDD テーマ

- Entity
- Value Object
- Domain Service
- Factory（必要になった場合のみ）

### レビュー観点

- setter だらけの Anemic Domain Model になっていないか
- 不正状態を簡単に生成できないか
- Domain Rule が Application Service へ流出していないか
- Value Object にする理由を説明できるか

---

## Step 4: Aggregate Boundary

ここが今回の重点学習領域。

### 自分で考えること

まず「一緒に取得したいもの」ではなく、以下から境界を考える。

> 1 Transaction で必ず整合していなければならないものは何か？

特に検討する。

- 受注と Operation は同一 Aggregate か
- Equipment は受注の Aggregate の中に入るのか
- Capacity を誰が所有するのか
- 複数の受注が 1 台の Equipment を取り合う場合、どこで整合性を守るか

### 読む DDD テーマ

- Aggregate
- Aggregate Root
- Invariant Boundary
- Aggregate 間は ID 参照する考え方
- Small Aggregate

### 必須レビュー

Codex へ以下を依頼する。

> 私の Aggregate Boundary をレビューしてください。特に、Invariant Boundary と Transaction Boundary から見て不自然な点、Aggregate が大きすぎる点、複数 Aggregate 間で即時整合性を要求してしまっている点を指摘してください。完成案は先に提示せず、まず問題点を指摘してください。

---

## Step 5: Transaction / Concurrency

Aggregate を作ったら、必ず Concurrency を考える。

### シナリオ

香山さんと別の生産管理担当者が同時に、同じ設備の同じ日に工程を追加する。

両者が変更前には「残り 8 時間」と読めていた場合、両方保存すると設備能力を超える可能性がある。

### 自分で考えること

- Aggregate だけで守れるか
- DB Transaction が必要か
- Optimistic Lock が必要か
- Serializable が必要か
- DB Constraint で表現可能か
- Capacity 超過を Invariant にすること自体が正しいか

### 読むテーマ

DDD だけでなく以下も読む。

- ACID Transaction
- Isolation Level
- Lost Update
- Optimistic Concurrency Control
- PostgreSQL row lock / Serializable 概要

### Exit Criteria

「同時更新されたらどうなる？」に具体的に答えられる。

---

## Step 6: Data Model

ここで初めて ER / Drizzle Schema を設計する。

### 自分で考えること

- Domain Model と Table を 1:1 にする必要があるか
- FK で保証できる整合性は何か
- UNIQUE / CHECK / NOT NULL で何を守るか
- 工程順序をどう保存するか
- スケジュールを履歴として残す必要があるか
- 日次負荷集計を都度計算するか保持するか

### 読むテーマ

- Repository
- Domain Model と Persistence Model の分離
- RDB Constraints
- Normalization
- Index

### 必須成果物

- Domain Model 図
- ER 図
- Drizzle Schema
- 「なぜ Domain Model と Data Model が違うか」の説明

---

## Step 7: Minimal Implementation

実装は設計確認のために最小限にする。

### Backend

最低限:

- Manufacturing Order 作成
- Operation 取得
- Operation を設備・日程へ割当
- Schedule 変更
- Capacity / Rule Validation

### Frontend

UI はモデルの不自然さを見つけるためだけに作る。

例:

- 受注一覧
- 受注詳細 + 工程一覧
- 設備別の日次スケジュール

ドラッグ&ドロップなどは不要。

### Test

CRUD Test より Domain Rule Test を優先する。

例:

- 前工程より前に後工程を配置できない
- 開始日 > 完了日の工程を作成できない
- Capacity Rule の境界値
- Concurrent Update の再現

---

## Step 8: Requirement Change Exercise

最小実装後に、意図的に要求変更を入れる。

候補:

1. 外注工程を追加する
2. 1 工程を複数設備で実行可能にする
3. 設備能力だけでなく作業者能力も考慮する
4. 日単位から時刻単位へスケジューリング精度を上げる
5. 工程を並列実行可能にする

変更前のモデルがどこまで耐え、どこから変更が必要なのかを確認する。

ここで初めて「将来拡張を考えた設計」が本当に必要だったか評価する。

---

## ADR

重要な設計判断は `docs/adr/` に残す。

最低限候補:

- Initial scheduling scope
- Aggregate boundaries
- Capacity consistency strategy
- Domain model vs persistence model

ADR には最低限以下を書く。

- Context
- Decision
- Alternatives
- Trade-offs
- Consequences

---

## Suggested Study Order

DDD を最初から最後まで通読しない。

必要になった概念を、その設計フェーズ直前に読む。

1. Use Case を作る前: Ubiquitous Language / Domain Rule
2. Domain Model 前: Entity / Value Object
3. Boundary 検討前: Aggregate / Invariant
4. Persistence 前: Repository
5. Concurrency 検討時: Transaction / Isolation（DDD 外）
6. 要求変更時: Domain Service / Domain Event などを必要に応じて読む

Domain Event、CQRS、Event Sourcing、Saga などは、現在の要求から必要性が出るまで読まなくてよい。

## Definition of Done for This Study

コード量では判断しない。

以下を自分の言葉で説明できれば完了とする。

- なぜこの Use Case を選んだか
- 主要な Domain Rule は何か
- Rule と Invariant の違い
- Entity / Value Object をどう判断したか
- Aggregate Boundary をなぜそこに置いたか
- Transaction Boundary との関係
- Concurrent Update をどう扱うか
- DB Constraint と Domain Logic の責務分担
- Domain Model と Data Model がどう違うか
- 外注工程を追加した場合、どこが変わるか
- 今でも自信がない設計論点は何か
