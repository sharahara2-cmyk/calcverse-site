# CalcVerse 実装ガイド：ゼロから自動収入まで

---

## 全体フロー

```
Phase 1: 基盤構築（1日目）
  ドメイン取得 → GitHub登録 → Cloudflare Pages デプロイ

Phase 2: コンテンツ拡充（1〜2週目）
  残りツールページ作成 → Google Analytics導入 → Search Console登録

Phase 3: 収益化（3〜4週目）
  AdSense申請 → 広告コード設置

Phase 4: SEO運用（継続）
  ツール追加 → 記事追加 → パフォーマンス監視
```

---

## Phase 1: 基盤構築（所要時間: 2〜3時間）

### Step 1: ドメイン取得

**推奨レジストラ**: Cloudflare Registrar（最安値。年間約1,200〜1,500円）

1. https://dash.cloudflare.com にアクセス
2. 無料アカウント作成（メールアドレス + パスワード）
3. 左メニュー「ドメイン登録」→「ドメインの登録」
4. 希望ドメインを検索（例: `calcverse.net`）
5. `.net` または `.com` を選択して購入

**注意**: `.net` は年間約1,200円、`.com` は約1,400円。
`.jp` は別レジストラが必要で割高なので非推奨。

**代替案**: 既に好みのドメインがある場合はそれでOK。
サイト内の `calcverse.net` 部分を自分のドメインに置換すること。

---

### Step 2: GitHubアカウント作成 + リポジトリ作成

1. https://github.com にアクセス → Sign up
2. 右上「+」→「New repository」
3. Repository name: `calcverse-site`
4. Public を選択
5. 「Create repository」をクリック

---

### Step 3: ファイルをGitHubにアップロード

**方法A: GitHub Web UI（最も簡単）**

1. 作成したリポジトリページで「uploading an existing file」をクリック
2. ダウンロードしたCalcVerseフォルダの中身をすべてドラッグ&ドロップ
3. 「Commit changes」をクリック

**フォルダ構造が維持されない場合**:
GitHub Web UIはフォルダ付きアップロードに制限がある。
その場合は方法Bを使用。

**方法B: GitHub Desktop（推奨）**

1. https://desktop.github.com からGitHub Desktopをダウンロード
2. インストール → GitHubアカウントでサインイン
3. File → Clone Repository → 先ほど作成した `calcverse-site` を選択
4. ローカルフォルダにCalcVerseのファイルをすべてコピー
5. GitHub Desktop左下「Summary」に「initial commit」と入力
6. 「Commit to main」→ 「Push origin」

---

### Step 4: Cloudflare Pagesでデプロイ

1. https://dash.cloudflare.com にログイン
2. 左メニュー「Workers & Pages」→「Pages」タブ
3. 「Create」→「Connect to Git」
4. GitHubアカウントを連携 → `calcverse-site` リポジトリを選択
5. 設定画面:
   - **Project name**: `calcverse`
   - **Production branch**: `main`
   - **Build command**: （空欄のまま）
   - **Build output directory**: （空欄のまま、または `/` ）
6. 「Save and Deploy」

**デプロイ完了まで1〜2分**。
`calcverse.pages.dev` のURLでサイトが確認できる。

---

### Step 5: カスタムドメイン設定

1. Cloudflare Pages → calcverse プロジェクト → 「Custom domains」タブ
2. 「Set up a custom domain」
3. `calcverse.net` を入力 → 「Continue」
4. DNSレコードが自動設定される → 「Activate domain」

**SSL/HTTPSは自動設定**。設定不要。
反映まで最大数分〜数十分。

---

## Phase 2: コンテンツ拡充（所要時間: 3〜5日）

### Step 6: 残りの計算ツールページ作成

同梱のBMIページ（`/bmi/index.html`）とローンページ（`/loan/index.html`）を
テンプレートとして、以下のページを作成する。

各ページで変更が必要な部分:
- `<title>` タグ
- `<meta name="description">` タグ
- `<link rel="canonical">` のURL
- パンくず
- h1タイトル
- 計算ロジック（JavaScript）
- 解説記事セクション（SEO用。最低500〜800文字）
- 構造化データ（FAQ + BreadcrumbList）

**優先順に作成すべきページ**（検索ボリューム順）:

| 優先度 | ツール | パス | 推定月間検索量 |
|--------|--------|------|---------------|
| ★★★ | 割り勘計算 | `/split/` | 高 |
| ★★★ | 消費税計算 | `/tax/` | 高 |
| ★★★ | 年齢計算 | `/age/` | 高 |
| ★★☆ | 複利計算 | `/compound/` | 中 |
| ★★☆ | 基礎代謝量 | `/calories/` | 中 |
| ★★☆ | 日数計算 | `/days/` | 中 |
| ★☆☆ | パーセント計算 | `/percentage/` | 中 |
| ★☆☆ | 速度計算 | `/speed/` | 低 |
| ★☆☆ | 単位変換(長さ) | `/unit-length/` | 低 |
| ★☆☆ | 単位変換(重さ) | `/unit-weight/` | 低 |

**ヒント**: Claudeに「割り勘計算のページをBMIページと同じ構造で作って」と
依頼すれば、個別ページを生成できる。

---

### Step 7: Google Analytics 導入

1. https://analytics.google.com にアクセス → Googleアカウントでログイン
2. 「測定を開始」→ アカウント名「CalcVerse」
3. プロパティ名「CalcVerse」→ 日本 / 日本円
4. ストリームの設定 → 「ウェブ」→ URL: `calcverse.net`
5. 測定IDが発行される（`G-XXXXXXXXXX` 形式）
6. 全HTMLファイルの `<head>` 内に以下を追加:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

`G-XXXXXXXXXX` を自分の測定IDに置換する。

---

### Step 8: Google Search Console 登録

1. https://search.google.com/search-console にアクセス
2. 「プロパティを追加」→「URLプレフィックス」→ `https://calcverse.net`
3. 所有権の確認: Cloudflareで管理している場合は「DNS レコード」が最も簡単
   - 表示されたTXTレコードをCloudflareのDNS設定に追加
4. 確認完了後、左メニュー「サイトマップ」→ `sitemap.xml` を送信
5. 「URL検査」→ トップページURLを入力 → 「インデックス登録をリクエスト」

---

## Phase 3: 収益化（デプロイから2〜4週間後）

### Step 9: Google AdSense 申請

**申請前チェックリスト**:
- [ ] 独自ドメインで公開済み
- [ ] 10ページ以上のコンテンツ（計算ツール12個 + 固定3ページ = 15ページ）
- [ ] プライバシーポリシー設置済み
- [ ] 利用規約設置済み
- [ ] お問い合わせページ設置済み
- [ ] サイト公開から2週間以上経過（推奨）

**申請手順**:
1. https://www.google.com/adsense にアクセス
2. 「ご利用開始」→ Googleアカウントでログイン
3. サイトURL: `https://calcverse.net`
4. 審査用コード（`<script>` タグ）を全ページの `<head>` に追加
5. GitHubにcommit → Cloudflare Pagesに自動デプロイ
6. AdSenseダッシュボードで「審査をリクエスト」

**審査期間**: 通常1〜2週間。不承認の場合は理由を確認して改善後に再申請。

---

### Step 10: 広告コード設置

承認後、AdSenseダッシュボードから広告ユニットを作成:

1. 「広告」→「広告ユニットごと」→「ディスプレイ広告」
2. 広告ユニット名: 「top-banner」
3. レスポンシブを選択 → コードをコピー

各HTMLファイルの `<div class="ad-slot">` 内にコードを貼り付け:

```html
<div class="ad-slot" id="ad-top">
  <ins class="adsbygoogle"
       style="display:block"
       data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
       data-ad-slot="XXXXXXXXXX"
       data-ad-format="auto"
       data-full-width-responsive="true"></ins>
  <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
</div>
```

**広告配置箇所**（各ツールページ共通）:
- ページ上部（計算ツールの上）: リーダーボード
- ページ中部（計算結果と記事の間）: レクタングル
- 記事下: レクタングル

---

## Phase 4: SEO運用（継続）

### 追加すべきツール（検索ボリューム重視）

| ツール名 | 推定キーワード | 備考 |
|----------|--------------|------|
| 年収→手取り計算 | 「年収 手取り」 | 検索量極めて高い |
| 電気代計算 | 「電気代 計算」 | 実用性高い |
| 排卵日計算 | 「排卵日 計算」 | 医療知識を活かせる |
| 妊娠週数計算 | 「妊娠週数 計算」 | 同上 |
| 体脂肪率計算 | 「体脂肪率 計算」 | BMIと関連 |
| 退職金計算 | 「退職金 計算」 | 検索量高い |
| ふるさと納税上限額 | 「ふるさと納税 上限」 | 季節需要あり |
| 相続税計算 | 「相続税 計算」 | 高単価広告が出やすい |

**金融・税務系は広告単価が高い**（CPC 100〜500円程度）。
医療系は単価はやや低いが検索量が安定している。

### SEOモニタリング

月1回以下を確認:
- Google Search Console: 検索クエリ、クリック数、掲載順位
- Google Analytics: ページ別PV、滞在時間
- AdSense: 収益、RPM、クリック率

### 更新サイクル

- 月2回: 新規ツール1つ追加
- 月1回: 既存ツールの解説記事を加筆
- 季節イベント前: 関連ツールを事前に公開（確定申告前に税金系など）

---

## 費用サマリー

| 項目 | 初年度 | 2年目以降 |
|------|--------|----------|
| ドメイン (.net) | 約1,200円/年 | 約1,200円/年 |
| Cloudflare Pages | 無料 | 無料 |
| GitHub | 無料 | 無料 |
| Google Analytics | 無料 | 無料 |
| Google Search Console | 無料 | 無料 |
| **合計** | **約1,200円/年** | **約1,200円/年** |

---

## 収益の現実的な見通し

| 期間 | 推定月間PV | 推定月間収益 |
|------|-----------|-------------|
| 1〜3ヶ月目 | 100〜1,000 | 0〜500円 |
| 4〜6ヶ月目 | 1,000〜10,000 | 500〜3,000円 |
| 7〜12ヶ月目 | 10,000〜50,000 | 3,000〜15,000円 |
| 2年目以降 | 50,000〜200,000 | 15,000〜80,000円 |

※ツール追加数、記事品質、SEO競合状況に大きく依存。
※金融系ツール（ローン・投資・税金）は広告単価が高い傾向。
※月間10万PV超えれば月3〜5万円は現実的。

---

## トラブルシューティング

### デプロイ後にサイトが表示されない
→ Cloudflare Pages の Build log を確認。
  ファイルがルート直下にあるか確認（フォルダの中にフォルダが入っていないか）。

### AdSense審査に落ちた
→ 最も多い理由:
  1. コンテンツ不足（各ページの解説記事を500文字以上に）
  2. ナビゲーション不備（全ページにヘッダー・フッターのリンクがあるか）
  3. ポリシー違反（プライバシーポリシーの記載漏れ）
  → 改善して2週間後に再申請。

### 検索結果に表示されない
→ Search Console の「URL検査」で各ページをインデックス登録リクエスト。
  sitemap.xml が正しく送信されているか確認。
  新規サイトはインデックスまで1〜4週間かかることがある。
