# bluebird — 楽曲特設LP(カナトチャット組み込み版)

オリジナル楽曲「bluebird」のランディングページ。右下の「カナトと話す」ボタンから、ボーカルのカナト本人とチャットできます。

## 構成

- `index.html` — LP本体(CSS/JS込み。ウィジェット読み込みタグ追加済み)
- `bluebird_demo.mp3` — デモ音源
- `kanato-widget.js` — チャットウィジェット(LPの配色・フォントに統一済み)
- `api/chat.js` — Vercelサーバーレス関数。**APIキーとカナトの人格設定はここ**

## 更新手順(既存のリポジトリに上書き)

既にGitHub連携でVercelにデプロイ済みの場合:

```bash
# このフォルダの中身を既存リポジトリに上書きコピーしてから
git add .
git commit -m "add Kanato chat"
git push
```

push すると自動で再デプロイされます。**ただし初回はAPIキーの設定が必要です(次項)。**

## APIキーの設定(初回のみ・必須)

1. https://console.anthropic.com でAPIキーを取得(従量課金。クレジット購入が必要)
2. 同コンソールの **Limits で月間利用上限(Spend Limit)を設定しておくことを強く推奨**
3. Vercelダッシュボード → プロジェクト → **Settings → Environment Variables** で
   - Key: `ANTHROPIC_API_KEY`
   - Value: 取得したキー(`sk-ant-...`)
4. **Deployments から Redeploy**(環境変数は再デプロイ後に反映されます)

これをしないとチャットは「接続に失敗しました」になります。LP自体は今まで通り表示されます。

## カスタマイズ

- **カナトの設定変更**: `api/chat.js` 冒頭の `PROFILE` を編集 → push
- **ボタン文言・挨拶の変更**: `index.html` のウィジェット読み込みタグの前に
  ```html
  <script>window.KANATO_CONFIG = { buttonLabel: "カナトと話す", firstMessage: "……" };</script>
  ```

## コスト・悪用対策

実装済み: 履歴は直近20メッセージまで / 1メッセージ1000文字まで / 応答は最大600トークン。
1往復あたり数円以下が目安ですが、公開後アクセスが増えたらVercelのFirewall(Rate Limiting)の設定を検討してください。

## 既存の注意事項(前バージョンから引き継ぎ)

- ビジュアル3枚とレコーディング写真は生成元CDNのURLを直接参照しています。恒久公開時は画像をダウンロードして `images/` に置き、`index.html` 内のURLを差し替えることを推奨
- ストリーミングリンク(`.links` の `href="#"`)は実URLに差し替え
