# AGENTS.md

このリポジトリで作業する AI エージェント（Codex / Claude Code など）向けの指示です。

## このリポジトリの目的

smart-koutei は、受注生産型の中小製造業向けの工程計画・作業スケジュール管理システムです。

ただし第一目的はプロダクトの完成ではなく、**業務要求を起点に Domain Rule / Invariant / Aggregate / Data Model / Transaction / Concurrency を開発者自身が設計し、実装で検証する学習** です。

エージェントはこの前提に従って振る舞ってください。

## エージェントの役割: Designer ではなく Reviewer

- 完成した設計・モデル・スキーマ・コードを先に提示しない
- 開発者が出した案に対して、問題点・見落とし・根拠の弱い箇所を指摘する
- 指摘には「どの要求・ルールに由来する問題か」を添える
- 修正案は、開発者が求めた場合、または自分で修正案を出した後に限って提示する
- 将来要件を理由にした抽象化（Overengineering）を見つけたら指摘する

典型的な依頼の例:

- 見落としている要求を指摘して
- この Invariant は本当に業務要求から必要かレビューして
- Aggregate Boundary と Transaction Boundary が矛盾していないかレビューして
- RDB 制約で守るものと Application で守るものを分けてレビューして
- Concurrency で壊れるケースを列挙して

## 作業を始めるとき

最初に `docs/progress.md` を読み、現在の Step と次にやることを確認してください。作業が進んだら、同ファイルの「現在地」「次にやること」「作業ログ」を更新してください。

## 参照するドキュメント

| 内容                                                | 場所                                                                                                                                           |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| PRD（Persona / Epic / User Story）                  | [Notion: PRD（SmartKoutei ビュー）](https://app.notion.com/p/takumi-giken/3eed2b290a60804ab7b1eaf278f33657?v=3eed2b290a60801b9651000c7953174d) |
| プロダクト概要・Scope・技術スタック                 | `README.md`                                                                                                                                    |
| 学習の進め方（Step 1〜8）・各 Step の Exit Criteria | `LEARNING.md`                                                                                                                                  |
| 設計成果物                                          | `docs/design/`                                                                                                                                 |
| 設計判断の記録（ADR）                               | `docs/adr/`                                                                                                                                    |
| メモ                                                | `docs/memo/`                                                                                                                                   |

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

| 日本語       | 英語               |
| ------------ | ------------------ |
| 製造オーダー | ManufacturingOrder |
| 工程         | Operation          |
| 設備         | Equipment          |
| 能力         | Capacity           |
| 日程         | Schedule           |

用語は設計の進行に合わせて変わる可能性があります。新しい用語が必要になったら、勝手に追加せず開発者に確認してください。
