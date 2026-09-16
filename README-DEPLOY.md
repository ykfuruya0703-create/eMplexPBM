# eMplex PBM サイト一式（デプロイ手引き）

## 構成（今回、ファイル名を index.html から変更しました）
- `index.html` … トップページ（LP本体。これは元々のファイルなのでこの名前のままです）
- `blog.html` … コラム一覧ページ（全86記事）
- `blog/{記事スラッグ}.html` … 記事ごとの独立ファイル
  例: `blog/forecast-management-01.html`, `blog/construction-industry-01.html` など
- `sitemap.xml`
- `robots.txt`

コラム記事は「フォルダ + index.html」ではなく、1記事=1つのHTMLファイルという
シンプルな構成にしました。これにより:
- ファイル名がすべて意味を持つ形になり、`index.html` が大量に並ぶ問題がなくなりました
- ローカルで `file://` として開いても、フォルダの自動解決に頼らず確実にリンクが機能します
- URLも `/blog/{記事スラッグ}.html` という分かりやすい形になります

## スラッグの命名規則
`{カテゴリの英語スラッグ}-{連番}` という形式です（例: `construction-industry-01`は建設業カテゴリの1本目）。
カテゴリ一覧:
- greeting（ご挨拶）/ forecast-management（フォーキャスト経営）/ project-profitability（プロジェクト収支管理）
- project-management-basics（プロジェクト経営管理基礎知識）/ timesheet（タイムシート）/ sales-support（営業支援）
- hr-management（人材マネジメント）/ corporate-governance（経営ガバナンス）/ fpa-management（FP&A・経営管理）
- ipo-preparation（IPO・上場準備）/ cost-accounting（原価計算実務）/ construction-industry（建設業）

記事ごとにさらに個別の意訳スラッグ（例:「jinmyaku-management」のような形）にしたい場合は、
`blog/` 以下のファイル名、各記事内の関連記事リンク、`blog.html`、`sitemap.xml`の
該当箇所をあわせて変更してください。

## 必ず確認・修正してほしい点

1. **ドメインの置き換え**（仮ドメイン: `https://www.emplex-pbm.com`）
   ```
   grep -rl "https://www.emplex-pbm.com" . | xargs sed -i 's#https://www.emplex-pbm.com#https://実際のドメイン#g'
   ```

2. **Google Search Console**
   `google-site-verification` メタタグは維持しています。
   サイトマップ登録: 「サイトマップ」→ `https://実際のドメイン/sitemap.xml`

3. **title / description / canonical**: 全86記事に個別設定済み
4. **Article構造化データ（JSON-LD）**: 全記事に埋め込み済み
5. **画像・ロゴ**: JSON-LDのpublisher.logoは `{DOMAIN}/assets/logo.png` のプレースホルダーです

## 動作確認方法
`site/index.html` をダブルクリックして開き、コラムのカードやタイトルをクリックして
`blog/{スラッグ}.html` に遷移することを確認してください。
`site/blog.html` を直接開いても、全記事の一覧から同様に遷移できます。
