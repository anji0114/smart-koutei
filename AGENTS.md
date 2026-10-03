# AGENTS.md

このリポジトリで作業する AI エージェント（Codex / Claude Code など）向けの指示です。

## このリポジトリの目的

smart-koutei は、受注生産型の中小製造業向けの工程計画・作業スケジュール管理システムです。

ただし第一目的はプロダクトの完成ではなく、**業務要求を起点に Domain Rule / Invariant / Aggregate / Data Model / Transaction / Concurrency を開発者自身が設計し、実装で検証する学習** です。

エージェントはこの前提に従って振る舞ってください。

## 役割分担

**開発者がドメインを理解し、設計を考えて判断する。エージェントはその考えを整え、書き起こし、レビューし、実装の細部を書く。**

### 開発者が行うこと

- 業務・ドメインの理解
- 要求・Use Case・Domain Rule・Domain Model・Aggregate Boundary などの設計の判断

### エージェントが行うこと

| 役割 | 内容 |
| --- | --- |
| 整える | 開発者の箇条書きや話し言葉のメモを、docs の形式に整理する |
| 書き起こす | 決まった内容を `docs/`（設計ファイル、ADR、`progress.md`、用語集）に書く |
| レビューする | 見落とし・矛盾・根拠の弱い箇所を、どの要求・ルールに由来するかを添えて指摘する |
| 実装する | 決まった設計に沿って、コードの細部（ボイラープレート、設定、テストコードなど）を書く |

### エージェントがしないこと

- 開発者が決めていない設計判断（Invariant、Aggregate Boundary、Transaction の方針など）を、先に決めて書く
  - 案が必要なときは、選択肢と Trade-off を示し、選ぶのは開発者に任せる
- 整えるときに、開発者が書いていない要求・ルール・用語を足す
  - 補った箇所や推測した箇所は、「（AI 補足）」のように明示する
- 実装中に、設計に書かれていない業務ルールを入れる
  - 設計に穴を見つけたら、実装で埋めずに開発者に報告する

レビューで特に見る観点:

- 見落としている要求はないか
- その Invariant は業務要求から本当に必要か
- Aggregate Boundary と Transaction Boundary が矛盾していないか
- RDB 制約で守るものと Application で守るものの分担
- Concurrency で壊れるケース
- 将来要件を理由にした抽象化（Overengineering）

## 作業を始めるとき

最初に `docs/progress.md` を読み、現在の Step と次にやることを確認してください。作業が進んだら、同ファイルの「現在地」「次にやること」「作業ログ」を更新してください。

## 参照するドキュメント

| 内容 | 場所 |
| --- | --- |
| PRD（Persona / Epic / User Story） | [Notion: PRD（SmartKoutei ビュー）](https://app.notion.com/p/takumi-giken/3eed2b290a60804ab7b1eaf278f33657?v=3eed2b290a60801b9651000c7953174d) |
| プロダクト概要・Scope | `README.md` |
| 学習の進め方（Step 1〜8）・各 Step の Exit Criteria | `LEARNING.md` |
| docs の地図と、どこに何を書くかのルール | `docs/README.md` |
| 現在地・次にやること | `docs/progress.md` |
| 用語集 | `docs/glossary.md` |
| 設計成果物 | `docs/design/` |
| 設計判断の記録（ADR） | `docs/adr/` |
| 論点ごとに読む本 | `docs/learning/books.md` |

新しいドキュメントを作るときは、`docs/README.md` のルールに従ってください。

要求について判断が必要なときは、推測せず Notion の PRD を確認してください。PRD と README が食い違う場合は、食い違いを開発者に報告してください。

## 進め方のルール

- `LEARNING.md` の Step の順序を守る。前の Step の Exit Criteria を満たす前に、後の Step（例: Data Model、実装）へ進めない
- Data Model / Drizzle Schema から設計を始めない。Domain Model と Data Model を区別する
- DDD パターン（Entity / Aggregate / Repository など）を、要求より先に当てはめない
- 現在の Vertical Slice の範囲外（README の「最初は扱わないもの」）を設計・実装に持ち込まない
- 事実・要求・設計判断を区別して書く。推測は推測と明記する
- 重要な設計判断は `docs/adr/` に ADR として残すよう促す（Context / Decision / Alternatives / Trade-offs / Consequences）

## 実装時のルール

実装は設計仮説を検証するためのもので、最小限にします。

- 技術スタック: `docs/design/architecture.md` を参照（TypeScript / Hono on Node.js / React + Vite / PostgreSQL / Drizzle / Hono RPC / pnpm workspaces / Vitest）
- 認証・インフラ・デザインシステムなど、設計学習に関係しない領域は扱わない
- テストは CRUD より Domain Rule（Invariant の境界値、工程順序違反、能力超過、同時更新）を優先する
- UI はモデルの不自然さを見つけるためだけに作る。ドラッグ&ドロップなどの作り込みはしない

## 用語

用語は `docs/glossary.md` に従ってください。新しい用語が必要になったら、勝手に追加せず開発者に確認してください。
