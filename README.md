# igai-gazou-koukai

@bebe1gu（bebe）のX画像付き投稿に使う**公開画像専用**リポジトリです。投稿本文、予約状態、認証情報はここには保存しません。

- 投稿管理（非公開・正本）：`N-GitJP/igai-note/bebe/posts/`
- 画像公開（このリポジトリ）：`images/YYYY/MM/<post_id>.png` など
- 配送：ChatGPT → GitHub投稿JSON → GAS → Buffer → X
- 課金、GitHub Actions、GitHub Pagesは不要です。通常の公開GitHub raw画像を利用します。

## 画像URLの作り方

例えば次のファイルをこのリポジトリの **main** に保存します。

```text
images/2026/10/example-post.png
```

Bufferへ指定する公開画像直リンク：

```text
https://raw.githubusercontent.com/N-GitJP/igai-gazou-koukai/main/images/2026/10/example-post.png
```

※ 上は**形式だけを示す例**であり、まだ画像ファイルはありません。GitHub上の`blob/main/...`ページURLではなく、`raw.githubusercontent.com`の**画像直リンク**を使います。

投稿の正本 `N-GitJP/igai-note/bebe/posts/YYYY-MM-DD/<post_id>.json` の `media` には、次のように設定します。

```json
{
  "media": {
    "type": "image",
    "url": "https://raw.githubusercontent.com/N-GitJP/igai-gazou-koukai/main/images/2026/10/example-post.png",
    "alt_text": "画像の比較対象、期間、数値を説明する文章"
  }
}
```

※画像を実際に保存し、一次情報・画像内数値・権利・公開URLを検証した場合だけ、元の投稿JSONの `verification.image_checked=true` とし、`ready` にします。

## 公開画像のルール

1. **PNG/JPEG/WebP、5MiB以下**。スマホでも読める画像を推奨。
2. **匿名でHTTP 200、正しいimage MIMEで直接GETできるURL**であること。期限付きURL、クエリ、リダイレクト、ログイン、HTML表示ページは使わない。
3. 画像ごとに固有ファイル名とパスを使う。予約後は**画像を差し替え・削除しない**（Bufferが後で取得する場合がある）。
4. 画像はオリジナルを基本とし、ニュース資料の画像・ロゴ・写真の利用権、数値、単位、比較軸、発表日時を確認する。
5. **このリポジトリは全世界へ公開される**。内部データ・未公開ニュース・個人情報・APIキー・トークン・投稿管理JSONは入れない。
6. 画像をアップロードできない場合、元の仕組みの**テキストのみ投稿を継続**。画像が指定された投稿を、勝手に画像なしで配信しない。

## GASの画像自動保存コード（2026-10-30）

既存の非公開管理リポジトリ `N-GitJP/igai-note` の `bebe/gas/Dispatcher.gs` に、編集者が一次情報で確認した`image_spec`の数値からGoogle Apps Script ChartsでPNGを作り、この公開リポジトリへGitHub Contents APIで保存する本番コードを追加しています。公開画像URLは元の投稿JSONに保存され、既存GAS→Buffer→Xの画像付き配信に使われます。

**GitHub内のコードは実装済みですが、稼働中GASへの最新コード反映は未確認です。** 本番反映が確認できるまで新規投稿への`image_spec`適用は保留します。Actionsと有料サービスは使いません。

## 現在の導入状態（2026-10-30 JST）

- 公開リポジトリ作成済み、読み書き権限を確認済み。
- 既存GASソースは画像URL添付に対応。ユーザーは2026-10-26にGAS本番反映完了と申告（稼働コード自体は未検証）。
- **画像の自動生成→PNG保存→GitHubアップロード経路はGASソース上で実装済み。稼働中GASへの反映は未確認。**
- **画像付きBuffer予約とX実公開の成功は未確認。**
- このREADMEやフォルダ作成は、投稿・予約・公開を発生させません。
