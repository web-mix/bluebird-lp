# bluebird — 楽曲特設LP

オリジナル楽曲「bluebird」のランディングページ。静的HTML 1枚 + デモ音源のみの構成で、ビルド不要です。

## 構成

- `index.html` — LP本体(CSS/JS込み)
- `bluebird_demo.mp3` — デモ音源(プレイヤーで再生)

## 公開手順(GitHub → Vercel)

1. GitHubで新規リポジトリを作成(例: `bluebird-lp`、Public/Privateどちらでも可)
2. このフォルダで以下を実行:

   ```bash
   git init
   git add .
   git commit -m "bluebird LP"
   git branch -M main
   git remote add origin https://github.com/<ユーザー名>/bluebird-lp.git
   git push -u origin main
   ```

3. [vercel.com](https://vercel.com) にGitHubアカウントでログイン → **Add New › Project** → `bluebird-lp` を **Import**
4. Framework Preset は **Other** のまま、Build設定は変更せず **Deploy**
5. `https://bluebird-lp-xxxx.vercel.app` のようなURLが発行されます(以後 `git push` するたび自動で再デプロイ)

## 注意: 画像のホスティングについて

現在、3枚のビジュアルは生成元(Higgsfield)のCDN URLを直接参照しています。将来リンク切れになる可能性があるため、恒久公開する場合は画像をダウンロードしてリポジトリに含めることを推奨します:

1. 各画像を保存し `images/` フォルダに配置(例: `hero.png` / `feather.png` / `dawn.png`)
2. `index.html` 内の `https://d8j0ntlcm91z4.cloudfront.net/...` のURL3箇所を `images/〇〇.png` に置換

## 更新したい箇所

- ストリーミングリンク: `index.html` 内 `.links` の `href="#"` を実URLに
- アーティスト名・リリース日: フッターやヒーローに追記可
