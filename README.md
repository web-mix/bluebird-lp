# bluebird — 楽曲特設LP(メンバーチャット組み込み版)

オリジナル楽曲「bluebird」のランディングページ。右下のチャットボタンから、バンドメンバー本人とチャットできます。

**当番は日替わり**(日本時間0時に交代): カナト(Vo.)→ショウゴ(Gt.)→ガク(Ba.)→ユウイチ(Dr.)の4日周期。挨拶・ボタン文言・人格がその日の当番に切り替わります。

## 構成

- `index.html` — LP本体(CSS/JS込み。ウィジェット読み込みタグ追加済み)
- `bluebird_demo.mp3` — デモ音源
- `kanato-widget.js` — チャットウィジェット(LPの配色・フォントに統一済み。日替わりの表示側ロジックもここ)
- `api/chat.js` — Vercelサーバーレス関数。**APIキーと4人分の人格設定はここ**(日替わりの人格側ロジックもここ。ウィジェットと同じ計算式で当番を決定)

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

- **メンバーの設定変更**: `api/chat.js` 冒頭の `BAND`(共通設定)と `MEMBERS`(個別の人格)を編集 → push。挨拶文は `kanato-widget.js` の `MEMBERS` に
- **日替わりをやめてカナト固定に戻す**: 両ファイルの `ROTATION` を `["kanato"]` にする
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
