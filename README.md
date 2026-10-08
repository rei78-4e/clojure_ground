# Clojure Playground

ブラウザだけで動く Clojure(Script) プレイグラウンド。単一の `index.html` のみで構成され、ビルド工程・バックエンドはありません。

- 評価: [Scittle](https://github.com/babashka/scittle) 0.8.33 (+ `cljs.pprint`)
- エディタ: CodeMirror 6 + `@nextjournal/lang-clojure`（import map で全パッケージのバージョンを固定）
- 実行: Run ボタン / `Ctrl+Enter`（macOS は `Cmd+Enter`）
- 共有: Share でコードを `deflate-raw` 圧縮 + base64url して URL ハッシュ (`#z=...`) に格納し、リンクをコピー
- スマホ（幅 800px 以下）: 下部に Code/Output タブと Run ボタン、編集中はキーボード直上に記号キー行（`( [ { " :` 、カーソル移動、インデント、Undo/Redo、フォーム選択拡張）
- 最後に編集したコードは `localStorage` に保存（共有リンクで開いた場合はリンクの内容が優先）

## ローカル確認

```sh
python3 -m http.server 8000   # http://localhost:8000
```

## Cloudflare Workers へのデプロイ (cplay.rei78.cc)

Workers の Static Assets で `index.html` をそのまま配信する。設定は `wrangler.jsonc`、公開から除外するファイルは `.assetsignore` に記述済み。

### A. Wrangler で直接デプロイ

```sh
npx wrangler login
npx wrangler deploy
```

`routes` の `custom_domain: true` により、`cplay.rei78.cc` の DNS レコードと証明書は自動で作成される（`rei78.cc` が同じ Cloudflare アカウントにあることが前提）。

### B. Git 連携（push で自動デプロイ）

1. Cloudflare ダッシュボード → **Workers & Pages** → **Create** → **Import a repository**
2. このリポジトリを選択し、次のように設定する
   - Build command: 空欄
   - Deploy command: `npx wrangler deploy`
   - Production branch: `master`
3. **Deploy**
