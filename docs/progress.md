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
| 1    | Use Case: 誰が何を判断する操作かを 3〜5 個に絞る                 | `docs/design/use-cases/`                     | 着手中 |
| 2    | Domain Rule / Invariant: 何が起きたら業務上おかしいかを列挙する  | `docs/design/domain-rules.md`                | 未着手 |
| 3    | Domain Model: ルールを自然に表現できるモデルを考える             | `docs/design/domain-model.md`                | 未着手 |
| 4    | Aggregate Boundary: 1 Transaction で整合すべき範囲を決める       | `docs/design/aggregates.md`、ADR             | 未着手 |
| 5    | Transaction / Concurrency: 同時更新で壊れないかを考える          | `docs/design/concurrency.md`、ADR            | 未着手 |
| 6    | Data Model: ER 図と Drizzle Schema を設計する                    | `docs/design/data-model.md`、ADR             | 未着手 |
| 7    | Minimal Implementation: 設計を検証する最小限の実装とテスト       | `server/`、`client/`                         | 未着手 |
| 8    | Requirement Change Exercise: 要求変更を入れてモデルの耐性を見る | `docs/design/change-exercise.md`             | 未着手 |

成果物のファイル名は目安。置き場所のルールは `docs/README.md` に従う。作るときに名前を変えたら、この表も直す。

各 Step の基本ループ（`LEARNING.md` の Learning Policy）:

1. 自分で考えて案を書く（箇条書きや雑なメモでよい）
2. AI に整えてもらう（内容は変えず、docs の形式にする）
3. AI にレビューさせる（完成案ではなく問題点を出させる）
4. 指摘がどの要求に由来するか確認し、自分で判断して直す
5. Exit Criteria を満たしたら次の Step へ

役割分担の詳細は `AGENTS.md` の「役割分担」を参照。

## 現在地

**Slice: Initial / Step 1: Use Case**

## 次にやること

- [ ] `docs/design/use-cases/README.md` の「どこまで書くか」「進め方」を読む
- [ ] 同 README の「候補の洗い出し」に候補を書き出す（質より量。10 個出てもよい）
- [ ] 3〜5 個に絞り、`_template.md` をコピーして 1 Use Case 1 ファイルで書く
- [ ] Scope 外にしたものを理由付きで書く
- [ ] AI にレビューを依頼する

## 作業ログ

新しいものを上に書く。

- 2026-10-03: AGENTS.md の役割分担を変更（開発者が判断し、AI は整える・書き起こす・レビュー・実装）。`docs/` のフォルダ構成を決定（ADR-0001、`docs/README.md`）。Use Case を 1 ファイル 1 件に分割。用語集を `docs/glossary.md` に移動
- 2026-10-03: AGENTS.md、`docs/design/architecture.md` を作成。技術スタックを確定（Hono on Node.js / React + Vite / PostgreSQL / Drizzle / Hono RPC / pnpm workspaces / Vitest）。Step 1 に着手
