# Progress

学習の全体の進め方と、現在地を記録する。セッションが切れたら、まずこのファイルを読んで再開する。

各 Step の詳しい内容と Exit Criteria は `LEARNING.md` を参照。

## 再開するとき

1. 下の「現在地」と「次にやること」を読む
2. 「現在地」の Step の作業ファイルを開く
3. 作業が進んだら、このファイルの「現在地」「次にやること」「作業ログ」を更新する

AI エージェントに再開を頼むときは「`docs/progress.md` を読んで続きから」と伝える。

## 全体の流れ

要求 → ルール → モデル → 境界 → 整合性 → DB → 実装 → 要求変更 の順で、1 本のユースケースを縦に通す。

| Step | やること                                                         | 成果物                                       | 状態   |
| ---- | ---------------------------------------------------------------- | -------------------------------------------- | ------ |
| 0    | 準備: README / AGENTS.md / アーキテクチャ                        | `README.md`、`AGENTS.md`、`docs/design/architecture.md` | 完了   |
| 1    | Use Case: 誰が何を判断する操作かを 3〜5 個に絞る                 | `docs/design/use-cases.md`                   | 着手中 |
| 2    | Domain Rule / Invariant: 何が起きたら業務上おかしいかを列挙する  | `docs/design/domain-rules.md`                | 未着手 |
| 3    | Domain Model: ルールを自然に表現できるモデルを考える             | `docs/design/domain-model.md`                | 未着手 |
| 4    | Aggregate Boundary: 1 Transaction で整合すべき範囲を決める       | `docs/design/aggregates.md`、ADR-002         | 未着手 |
| 5    | Transaction / Concurrency: 同時更新で壊れないかを考える          | `docs/design/concurrency.md`、ADR-003        | 未着手 |
| 6    | Data Model: ER 図と Drizzle Schema を設計する                    | `docs/design/data-model.md`、ADR-004         | 未着手 |
| 7    | Minimal Implementation: 設計を検証する最小限の実装とテスト       | `server/`、`client/`                         | 未着手 |
| 8    | Requirement Change Exercise: 要求変更を入れてモデルの耐性を見る | `docs/design/change-exercise.md`             | 未着手 |

成果物のファイル名は目安。作るときに変えてよい（変えたらこの表も直す）。

各 Step の基本ループ（`LEARNING.md` の Learning Policy）:

1. 自分で案を書く
2. AI にレビューさせる（完成案ではなく問題点を出させる）
3. 指摘がどの要求に由来するか確認する
4. 自分で直す
5. Exit Criteria を満たしたら次の Step へ

## 現在地

**Step 1: Use Case**

## 次にやること

- [ ] `docs/design/use-cases.md` の「やること」を読む
- [ ] Use Case の候補を自分で書き出す（質より量。10 個出てもよい）
- [ ] 3〜5 個に絞り、それぞれ Actor / Input / Decision / Constraint / Output を埋める
- [ ] Scope 外にしたものを理由付きで書く
- [ ] AI にレビューを依頼する

## 作業ログ

新しいものを上に書く。

- 2026-10-03: AGENTS.md、`docs/design/architecture.md` を作成。技術スタックを確定（Hono on Node.js / React + Vite / PostgreSQL / Drizzle / Hono RPC / pnpm workspaces / Vitest）。Step 1 に着手
