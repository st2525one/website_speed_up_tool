# ホームページ速度改修：AI作業手順・検証キット

匿名・汎用版。調査、修正候補、Docker検証、計測、報告、本番反映までの手順をまとめています。特定案件の実績報告ではありません。

実案件の会社名・顧客名・人名、サイトURL、ローカルパス、ファイル一覧、試験ID、日時、ハッシュ、実測値、固有の件数・契約条件は収録していません。本文中の技術・ソフトウェア名と公式資料へのリンクは、手順を理解するための一般情報です。例示URL・入力値は架空のものとして扱います。

## 最初に使う3ファイル

1. [初動の質問票](templates/01_intake.md)：困っている画面、守る動作、権限、受領物を確認する。
2. [AIへの開始指示](templates/02_ai_start.md)：操作範囲、秘密情報、成果物、合格条件を決める。
3. [測定計画](templates/04_measurement_plan.md)：対象、回線、キャッシュ、反復数、完了条件をそろえる。

## 手順書

| 作業 | 資料 |
| --- | --- |
| 着手・範囲決定 | [作業の全体像](playbook/01_start.md) |
| 公開サイトの調査 | [監査手順](playbook/02_audit.md) |
| 配信・画像・JS・動画の改修 | [修正の判断](playbook/03_fixes.md) |
| PHPの点検・互換性 | [PHP検証](playbook/04_php.md) |
| Dockerで前後を再現 | [検証環境](playbook/05_docker.md) |
| 低速回線での速度比較 | [測定手順](playbook/06_measurement.md) |
| 機能・画面を保つ | [UI検証](playbook/07_ui_qa.md) |
| AIへの指示・レビュー・引継ぎ | [AIの使い方](playbook/08_ai_workflow.md) |
| 秘密情報・本番の保護 | [情報の扱い](playbook/09_security.md) |
| ベンダー調整・費用・保守 | [適用と報告](playbook/10_delivery.md) |
| 問題を切り分ける | [つまずき一覧](playbook/11_troubleshooting.md) |
| 追加のSEO/AEO | [追加施策](playbook/12_seo_aeo.md) |
| コマンド例 | [実務レシピ](playbook/13_recipes.md) |

## 確認用の一覧と例

- [ボトルネック・修正・検証のチェック項目](examples/01_bottlenecks.md)
- [実測値を使わない報告例](examples/02_reporting_example.md)
- [空テンプレート10種](templates/README.md)
- [匿名化の範囲](evidence/PRIVACY.md)
- [収録範囲](evidence/COVERAGE.md)／[参考資料](evidence/SOURCES.md)／[ファイル検査](evidence/VERIFICATION.md)

## 使い方

手順をコピーし、案件専用の管理領域で対象URL・版・担当を埋めます。共有用の匿名資料と、実データを持つ作業台帳は分けてください。新しい案件で記入した会社名やパスは、外部共有前に再度点検します。

このZIPはMarkdownのみです。サイトの実コード、画像・動画、DB、認証値、実結果JSON、実行用コンテナは含みません。
