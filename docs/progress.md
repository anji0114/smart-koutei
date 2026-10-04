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
| 1    | Use Case: 誰が何を判断する操作かを 3〜5 個に絞る                 | `docs/design/use-cases/`、`scope.md`、ADR-0002 | 完了   |
| 2    | Domain Rule / Invariant: 何が起きたら業務上おかしいかを列挙する  | `docs/design/domain-rules.md`、ADR-0003      | 完了   |
| 3    | Domain Model: ルールを自然に表現できるモデルを考える             | `docs/design/domain-model.md`                | 着手中 |
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

**Slice: Initial / Step 3: Domain Model**

## 次にやること

- [ ] `docs/design/domain-model.md` の「やること」を読む
- [ ] 概念ごとに「何を知っているか」「何を判断・計算するか」を書く（箇条書きでよい。AI が整える）
- [ ] R1〜R5 を誰が守る・計算するかを決める
- [ ] Entity / Value Object を理由つきで決める
- [ ] AI にレビューを依頼する

## 作業ログ

新しいものを上に書く。

- 2026-10-04: Step 2 完了。ルールの強さを決定（ADR-0003: R1・R2 は Invariant、R3 は計算の仕方、R4 は警告、R5 は仮定、R6 は次の Slice）。「製造オーダー」をやめて「受注」に統一、工程 1 つ分は「割当」と呼ぶ。完了ボタン（UC-02）は次の Slice。Step 3 の作業シートを作成
- 2026-10-04: 開発環境のバージョンを Node.js 24、pnpm `>=11.0.0 <12` に決定。architecture.md に反映。導入は未実施
- 2026-10-04: Step 1 完了。Slice を確定（ADR-0002、README 更新）。C2（能力時間）も「追加時は禁止、後から破れたら警告」に変更。Step 2 の作業シート `docs/design/domain-rules.md` を作成
- 2026-10-03: UC-01（スケジュールを作成する）をレビュー済みにした。Constraint は C1（正規スケジュールは 1 本）、C2（設備の 1 日の能力時間）、C4（日をまたぐ。設備ごとに順番を持つ）、C5（依存。追加時は禁止、ずれで破れたらアラート）。連鎖の論点（順番の決め方、設備の空き日、受注をまたぐずれ）は Step 2 へ
- 2026-10-03: 元メモを操作ごとに分割（事務: 受注登録 / 課長: スケジュール作成）。スコープが広すぎるため `docs/design/scope.md` で絞り込みを開始。テンプレートに事前条件を追加
- 2026-10-03: AGENTS.md の役割分担を変更（開発者が判断し、AI は整える・書き起こす・レビュー・実装）。`docs/` のフォルダ構成を決定（ADR-0001、`docs/README.md`）。Use Case を 1 ファイル 1 件に分割。用語集を `docs/glossary.md` に移動
- 2026-10-03: AGENTS.md、`docs/design/architecture.md` を作成。技術スタックを確定（Hono on Node.js / React + Vite / PostgreSQL / Drizzle / Hono RPC / pnpm workspaces / Vitest）。Step 1 に着手
