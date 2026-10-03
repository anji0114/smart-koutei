# ADR（Architecture Decision Records）

設計上の重要な判断と、その理由を残す。

## 一覧

| ADR                                            | タイトル             | Status   | 日付       |
| ---------------------------------------------- | -------------------- | -------- | ---------- |
| [0001](./0001-document-placement.md)           | ドキュメントの置き場所 | Accepted | 2026-10-03 |

## 書くとき

- 書くべきもの: 後から「なぜこうなっている？」と聞かれそうな判断。選択肢が複数あり、どれかを選んだもの
- 書かなくてよいもの: 選択肢がほぼ 1 つしかないもの、すぐに変えられるもの
- 一度書いた ADR は原則書き換えない。判断を変えるときは新しい ADR を書き、古い ADR の Status を `Superseded by ADR-XXXX` にする
- ファイル名: `NNNN-kebab-case-title.md`

## テンプレート

```markdown
# ADR-NNNN: タイトル

- **Status**: Proposed / Accepted / Superseded by ADR-XXXX
- **Date**: YYYY-MM-DD

## Context

何が問題で、なぜ今決める必要があるか。関係する要求・制約。

## Decision

何を決めたか。

## Alternatives

検討した他の選択肢。

## Trade-offs

選んだ案の利点と、諦めたこと。

## Consequences

この判断によって、今後何が変わるか・何に気をつける必要があるか。
```
