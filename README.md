# notes

個人的な学習ノートと、その公開用サイト。分野は問わない（情報技術・物理学・数学など）。

## ディレクトリ構成

```
notes/
├── app/              Cloudflare Pages で配信する静的サイト
└── reference/        執筆・デザイン時に参照する外部資料
```

### app/

静的サイト。ビルドは不要で、HTML をそのまま配信する。ディレクトリ名がそのまま URL のパスになる（Next.js App Router と同じ考え方）。

```
app/index.html                →  /
app/<page-name>/index.html    →  /<page-name>/
```

`app/_headers` は Cloudflare Pages のレスポンスヘッダー設定で、全ページにセキュリティヘッダーを付与している。`Strict-Transport-Security` だけは Cloudflare 側（SSL/TLS → Edge Certificates）で設定しているため、このファイルには含めない。

### reference/

外部資料の置き場。リポジトリの成果物ではなく、参照専用。

## 運用

- ページは `app/<page-name>/index.html` として追加する
- ディレクトリ名は英小文字とハイフン（例: `jamstack-history`）
- ページを追加したら、目次（`app/index.html`）からリンクする
- 各ページは単一の HTML で完結させる。CSS は `<style>` に含める
- 出典は文末にリンクで残す

## ローカルプレビュー

Cloudflare Pages と同じ配信エンジンで確認する。

```sh
npx wrangler pages dev app
```

http://127.0.0.1:8788/ で確認できる。

## Cloudflare Pages の設定

| 項目 | 値 |
| --- | --- |
| フレームワークプリセット | なし（None） |
| ビルドコマンド | 空欄 |
| ビルド出力ディレクトリ | `app` |

出力ディレクトリを `app` に指定しないと、リポジトリ直下が配信されて `/app/...` のような URL になる。

### 知っておくべき挙動

- **存在しないパスには `/index.html` が HTTP 200 で返る**。`404.html` を置いていないため、Cloudflare Pages のフォールバックが働く。応答コードだけではファイルの有無を判断できない
- ディレクトリ形式のページは、末尾スラッシュなしの URL（`/page-name`）から末尾スラッシュあり（`/page-name/`）へ 308 リダイレクトされる

## デザイン方針

新しいページはデジタル庁デザインシステム（DADS）に準拠して作る。

- 書体は Noto Sans JP / Noto Sans Mono
- 本文は 16 CSS px 以上（14 px 未満は使わない）
- 行高は本文 160〜175%、見出し 140〜150%
- テキストのコントラスト比 4.5:1 以上、罫線など非テキスト要素は 3:1 以上
- フォーカスインジケーターは Yellow-300 と Black の 2 重構造

過去に作ったページには独自デザインのものもあるため、既存ページを変更するときは対象のスタイルを確認してから手を入れる。

## 出典

`reference/dads-markdown-20260909/` は、デジタル庁デザインシステムβ版サイトのドキュメントを Markdown 化した公式アーカイブを、そのまま収録したもの。2026年9月9日版。

> 出典：デジタル庁デザインシステムウェブサイト https://design.digital.go.jp/dads/

`app/` 配下のスタイルには、上記デザインシステムをもとに作成したものが含まれる。

> デジタル庁デザインシステムウェブサイト https://design.digital.go.jp/dads/ のコンテンツを加工して作成

カラー値は公式のデザイントークン [@digital-go-jp/design-tokens](https://github.com/digital-go-jp/design-tokens) v2.0.1（MIT License, Copyright (c) 2023 デジタル庁）を参照した。

いずれもデジタル庁が作成したものではない。
