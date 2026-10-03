# docs

このフォルダの地図と、どのドキュメントをどこに置くかのルール。新しいドキュメントを作るときは、まずここを見る。

## 置き場所の大原則

| 種類 | 置き場所 | 理由 |
| --- | --- | --- |
| 要求（Persona / Epic / User Story） | Notion PRD | 「何を・なぜ」はビジネス側の言葉で管理し、共有しやすくする |
| 設計・判断・進捗・学習記録 | このリポジトリの `docs/` | AI エージェントが直接読める。差分でレビューでき、コードと一緒に履歴が残る |

同じ情報を Notion とリポジトリの両方に書かない。必要なら片方からリンクする。

## 構成

```text
docs/
├── README.md              # この文書（地図とルール）
├── progress.md            # 現在地・次にやること・作業ログ（セッション再開の入口）
├── glossary.md            # 用語集（ユビキタス言語）
├── design/                # 設計成果物: 「今の正しい設計」
│   ├── architecture.md
│   ├── use-cases/         # Use Case（1 ファイル 1 Use Case）
│   │   ├── README.md      #   一覧・書き方
│   │   ├── _template.md
│   │   └── UC-01-xxx.md
│   ├── domain-rules.md    # Step 2 で作る
│   ├── domain-model.md    # Step 3 で作る
│   └── ...
├── adr/                   # 設計判断の記録: 「その時点でなぜそう決めたか」
│   ├── README.md          #   一覧・テンプレート
│   └── 0001-xxx.md
└── learning/              # 学習の記録（設計そのものではないもの）
    ├── books.md           #   論点ごとに読む本
    └── notes/             #   論点ごとの学びメモ（必要になったら作る）
```

まだ存在しないファイル・フォルダは、その Step に入ったときに作る。先に空のファイルを用意しない。

## フォルダごとのルール

### `design/`: 今の正しい設計

- 常に最新の状態にする。考えが変わったら上書きする（古い版は git の履歴に残る）
- ファイルは **Step 名ではなく中身の種類** で分ける。Domain Model は Step 3 で作り、Step 4 や Step 8 で書き換えるため
- 1 ファイルが長くなったら、フォルダにして 1 項目 1 ファイルに分ける（`use-cases/` がその例）
- 各ファイルには、AI や他者からのレビュー指摘と対応を書く「レビュー記録」の節を持たせてよい

### `adr/`: 判断の記録

- 「なぜそう決めたか」を残す。Alternatives と Trade-offs を必ず書く
- 一度書いた ADR は原則書き換えない。判断を変えるときは、新しい ADR を書き、古い ADR の Status を `Superseded by ADR-XXXX` にする
- 番号は 4 桁の連番（`0001-`）。ファイル名は `NNNN-kebab-case-title.md`

`design/` と `adr/` の関係: `design/` は結論だけを書き、迷った末の判断理由は ADR に書いてリンクする。

### `learning/`: 学習の記録

- 本のガイド、読んだ内容のメモ、理解が曖昧な論点など
- プロダクトの設計としては参照しない（設計に反映すべき学びは `design/` か `adr/` に書く）

### `progress.md` と `glossary.md`

- `progress.md`: 作業を始めるとき・終えるときに必ず更新する
- `glossary.md`: 新しい業務用語が出てきたら追加する。docs とコードの命名はここに合わせる

## ファイル名の規則

- 英小文字の kebab-case（例: `domain-rules.md`）
- 連番を持つもの: Use Case は `UC-NN-title.md`、ADR は `NNNN-title.md`
- フォルダの説明は、そのフォルダの `README.md` に書く

## Slice が増えたとき

いまは Initial Development Slice だけを扱っている。次の Slice に進んでも、フォルダは Slice ごとに分けない。設計は Slice をまたいで 1 つのモデルとして育てるため。

- Use Case には「どの Slice で追加したか」を書く
- `progress.md` の「現在地」に、どの Slice のどの Step かを書く
