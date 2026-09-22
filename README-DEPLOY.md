# eMplex PBM サイト一式（デプロイ手引き）

## ローカルでの確認について
すべての内部リンクに `index.html` を明示的に含めています。
`file://` でindex.htmlを直接開いた場合、ブラウザはフォルダ指定（例: `blog/xxx/`）だけでは
中のindex.htmlを自動的に開いてくれません（これはWebサーバーが提供する挙動で、
ローカルファイルにはその機能がないためです）。
そのため `blog/xxx/index.html` のように拡張子まで明記したリンクにしてあります。
これでローカルのダブルクリックでも、本番のサーバーでも問題なく開けます。

## 中身（このフォルダの中身をそのままドメイン直下に置いてください）
- `index.html` … トップページ
- `blog/index.html` … コラム一覧ページ（全86記事）
- `blog/{slug}/index.html` … 記事ごとの独立ページ（例: `/blog/forecast-management-01/index.html`）
- `sitemap.xml`
- `robots.txt`

補足: サイトマップやcanonicalタグには `https://.../blog/{slug}/`（末尾スラッシュ、
index.htmlなし）のクリーンなURL形式を使っています。一般的なWebサーバーでは
`/blog/{slug}/` へのアクセス時に自動的にフォルダ内の `index.html` を返すため、
検索エンジンにはこちらの短いURLを見せつつ、サイト内のリンクは
`index.html` 付きの確実な形にする、という使い分けです。
お使いのサーバーがこの自動解決に対応していることを確認してください
（Apache/Nginx/多くのホスティングサービスは標準で対応しています）。

## 必ず確認・修正してほしい点

1. **ドメインの置き換え**
   ```
   grep -rl "https://www.emplex-pbm.com" . | xargs sed -i 's#https://www.emplex-pbm.com#https://実際のドメイン#g'
   ```

2. **URLスラッグ**
   `{カテゴリ英語スラッグ}-{連番}` 方式です（例: `construction-industry-01`）。
   記事ごとの完全な意訳スラッグまではつけていません。変更する場合は
   `blog/` 以下のフォルダ名、関連記事リンク、`blog/index.html`、`sitemap.xml`を
   まとめて変更してください。

3. **Google Search Console**
   `google-site-verification` メタタグは維持しています。
   サイトマップ登録: 「サイトマップ」→ `https://実際のドメイン/sitemap.xml`

4. **title / description / canonical**: 全86記事に個別設定済み
5. **Article構造化データ（JSON-LD）**: 全記事に埋め込み済み
6. **画像・ロゴ**: JSON-LDのpublisher.logoは `{DOMAIN}/assets/logo.png` のプレースホルダーです

## 動作確認方法
`site/index.html` をダブルクリックして開き、コラムのカードやタイトルをクリックして
記事ページ（例: `blog/forecast-management-01/index.html`）に遷移することを確認してください。
