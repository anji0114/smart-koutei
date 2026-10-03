# ADR-0001: ドキュメントの置き場所

- **Status**: Accepted
- **Date**: 2026-10-03

## Context

- 要求（Persona / Epic / User Story）は、すでに Notion の PRD データベースで管理している
- このプロジェクトは継続的に進める学習で、「自分で書く → AI がレビュー・整理する → 直す」を繰り返す
- AI エージェント（Claude Code / Codex）が設計ドキュメントを読み書きする必要がある
- 設計は Step を進むたびに書き換わる（例: Domain Model は Step 3 で作り、Step 4・8 で変わる）

## Decision

- 要求は Notion PRD、設計・判断・進捗・学習記録はリポジトリの `docs/` に置く
- `docs/` は中身の種類で分ける: `design/`（今の正しい設計）、`adr/`（判断の記録）、`learning/`（学習の記録）
- Step ごと・Slice ごとのフォルダは作らない
- 詳しいルールは `docs/README.md` に書く

## Alternatives

1. **すべて Notion に置く**: 共有・閲覧はしやすいが、AI エージェントからの読み書きに Notion 連携が必要で、差分レビューもしづらい
2. **すべてリポジトリに置く**: 一元化できるが、すでに Notion にある PRD を移す手間がかかり、ビジネス側の言葉で書く要求と設計が混ざる
3. **`docs/` を Step ごとに分ける**（`01-use-cases/` など）: 学習の順序とは一致するが、後の Step で書き換えた設計の最新版がどこにあるか分からなくなる

## Trade-offs

- 利点: AI が直接読める。git の diff で設計の変化を追える。設計とコードを同じコミットで変えられる
- 諦めたこと: 要求と設計が別の場所にあるため、両方を見るときは行き来が必要。Notion での閲覧性

## Consequences

- 同じ情報を Notion とリポジトリの両方に書かない。必要ならリンクする
- 新しいドキュメントを作るときは `docs/README.md` のルールに従う
