# Clojure Playground

ブラウザだけで動く Clojure(Script) プレイグラウンド。単一の `index.html` のみで構成され、ビルド工程・バックエンドはありません。

- 評価: [Scittle](https://github.com/babashka/scittle) 0.8.33 (+ `cljs.pprint`)
- エディタ: CodeMirror 6 + `@nextjournal/lang-clojure`（import map で全パッケージのバージョンを固定）
- 実行: Run ボタン / `Ctrl+Enter`（macOS は `Cmd+Enter`）
- 共有: Share でコードを `deflate-raw` 圧縮 + base64url して URL ハッシュ (`#z=...`) に格納し、リンクをコピー
- 最後に編集したコードは `localStorage` に保存（共有リンクで開いた場合はリンクの内容が優先）

## ローカル確認

```sh
python3 -m http.server 8000   # http://localhost:8000
```

## Cloudflare Pages へのデプロイ (cplay.rei78.cc)

### A. Git 連携（push で自動デプロイ）

1. Cloudflare ダッシュボード → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. このリポジトリを選択し、ビルド設定を次のようにする
   - Framework preset: `None`
   - Build command: 空欄
   - Build output directory: `/`
3. **Save and Deploy**
4. 作成したプロジェクト → **Custom domains** → **Set up a custom domain** → `cplay.rei78.cc`
   - `rei78.cc` が同じアカウントの Cloudflare DNS にあるため、CNAME レコードは自動で作成される

### B. Wrangler で直接アップロード

```sh
npx wrangler pages project create clojure-playground --production-branch master
npx wrangler pages deploy . --project-name clojure-playground
```

その後、A の手順 4 と同様にカスタムドメイン `cplay.rei78.cc` を追加する。
