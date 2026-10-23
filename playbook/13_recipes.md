# 13 次の案件で使う最小の確認レシピ

以下は架空の対象を使った実行例。特定案件の実行記録ではない。`example.invalid`等は、許可された対象に置き換える。まず少数のファイル・代表URLから確認する。秘密ファイルを一括取得・検索・表示するコマンドは使わない。

## 公開CSSの応答と内容を分けて保存する

```bash
mkdir -p audit/http
# 認証不要と確認した公開の静的CSSだけを対象にする。
ASSET_URL='https://example.invalid/assets/css/style.css'
curl --fail --silent --show-error --max-time 60   -H 'Accept-Encoding: identity'   -D audit/http/style.identity.headers   -o audit/http/style.identity.css "$ASSET_URL"
curl --fail --silent --show-error --max-time 60 --compressed   -D audit/http/style.compressed.headers   -o audit/http/style.decoded.css "$ASSET_URL"
shasum -a 256 audit/http/style.identity.css audit/http/style.decoded.css
```

`--compressed`の保存本文は解凍済み。保存ファイルの大きさを転送バイト数とは呼ばない。圧縮方式は応答ヘッダーで確認する。静的公開資産でもCookie等が返る場合があるため、保存ヘッダーをログへ丸ごと貼らず、共有時は許可した項目だけ抽出する。リダイレクトがあれば、移動先を確認してから対象を決め直す。

## gzipはローカル試算として実行する

```bash
gzip -6 -n -c audit/http/style.identity.css > audit/http/style.gzip6.gz
gzip -d -c audit/http/style.gzip6.gz > audit/http/style.roundtrip.css
shasum -a 256 audit/http/style.identity.css audit/http/style.roundtrip.css
wc -c audit/http/style.identity.css audit/http/style.gzip6.gz
```

圧縮レベル6、タイムスタンプ・元ファイル名を含めない `-n`、gzipの実装/版を記録する。復元SHA一致は内容同一の検査、byte差はサイズ試算。実サーバーでgzipが有効になった証明やページが何秒速くなるかの測定ではない。

## 受領物を読む順序

1. まずファイル名・拡張子・サイズの一覧を作る。認証・設定・フォーム・ダンプを分類し、読取対象を許可リストにする。
2. 許可済みPHPについて、include、get_header/footer、クエリ、画像出力、資産URL、外部呼出しを調べる。
3. 行番号付きの発見を課題票へ記録する。関数名の出現だけで性能不具合とはしない。
4. SHAを採り、原本の変更を禁止する。ZIPは許可リストから作る。

秘密ファイルを含み得るディレクトリへ `rg` や `cat` を無差別にかけ、その出力をAIへ渡さない。既存の値なし検出記録がある場合はそれを使う。

## PHPの最小確認

```bash
# すでに用意した検証用Dockerサービスと許可済みPHPだけを指定する。
# サービス名・ファイル名は対象環境に合わせる。
docker compose exec -T php-check php -v
docker compose exec -T php-check php -l /work/allowed-template.php
```

PHP版、拡張、ini、WP/プラグイン、DBを記録する。`php -l`は第一段階にすぎない。実行ハーネスでは、警告・例外時に非0終了することを故意の失敗で確認し、通常値・空値・型違いを流す。実フォーム、メール、外部APIを無効にした範囲も書く。

## Dockerを再開するとき

`docker compose ps`と対象プロジェクト名で既存状態を確認する。用途不明のコンテナやvolumeを一括削除しない。`docker compose config`は展開した秘密値を出す可能性があるため、そのまま共有ログへ残さない。

before/afterの応答、バージョン、対象ページの機能がそろってから測定する。元のPHPだけのZIPでは、共有資産・仮データ・基盤が欠けて再現できないことがある。必要物のmanifestを確認する。

## 測定前の最後の確認

- 初回/再アクセスの独立したcontext設計と、Playwright routeを使っていないこと。
- 原本・候補・観測コードのSHA、版、予定アクセス数。
- 低速設定の校正、動画/表紙の準備判定、timeoutと除外条件。
- 外部通信・本番送信の遮断と、前後で同じ省略。
- 手動ビューアーや他のベンチマークが動いていないこと。
- 記録の保存先、失敗時の保存、UIを閉じても続く実行方法。

測定後は、入力SHA不変、各セルn、除外理由、独立中央値、原データSHAを確認してから報告する。
