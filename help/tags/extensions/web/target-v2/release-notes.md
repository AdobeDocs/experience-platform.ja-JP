---
title: Adobe Target v2 拡張機能のリリースノート
description: Adobe Experience Platform の Adobe Target v2 タグ拡張機能の最新のリリースノートです。
exl-id: c1a04e62-026d-4b16-aa70-bc6d5dbe6b2d
source-git-commit: 824fea41bc7e7082814648efd58184f5208e5e6f
workflow-type: ht
source-wordcount: '642'
ht-degree: 100%

---

# Adobe Target v2 拡張機能のリリースノート

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 2022年1月28日（PT）

### Adobe Target v2 拡張機能 0.17.1

- `at.js` v2.8.1 をサポートするように更新しました
- `pageLoad` が ODD ハイブリッド実行モードの `target-global-mbox` にマッピングされない問題を修正しました
- `mbox` リクエストの分析の詳細に関する問題を修正しました
- 開発の依存を更新して、セキュリティの脆弱性を修正しました。

## 2022年1月7日（PT）

### Adobe Target v2 拡張機能 0.17.0

- `at.js` v2.8.0 をサポートするように更新されました。これにより、機能の使用状況とパフォーマンスのテレメトリデータを収集するようになりました。個人データは収集されません。この機能をオプトアウトするには、`targetGlobalSettings` で `telemetryEnabled` を `false` に設定します。

## 2021年10月28日（PT）

### Adobe Target v2 拡張機能 0.16.0

- `at.js` v2.7.0 がサポートされるように更新されました。これにより、Adobe Target からダウンロードできるようになりました。

## 2021年7月20日（PT）

### Adobe Target v2 拡張機能 0.15.1

- `stringify` 関数名のクラッシュが原因で、`sessionId` や `requestId` などに対して誤った UUID 値が生成されていた問題を修正しました。

## 2021年7月16日（PT）

### Adobe Target v2 拡張機能 0.15.0

- `at.js` 設定の secureOnly が true に設定されている場合は必ず、cookie に secure 属性を追加します
- `triggerView()` を使用する際にレスポンストークンを使用できるようになりました
- `CONTENT_RENDERING_NO_OFFERS` イベントに関連するバグを修正しました。Target からコンテンツが返されない場合でも、常に正しくトリガーされるようになりました
- プリフェッチリクエストを使用したときに、A4T のクリック指標の詳細が正しく返されるようになりました
- UUID の生成で `Math.random()` の使用を終了し、`window.crypto` に基づくようになりました
- `sessionId` cookie の有効期限は、すべてのネットワーク呼び出しで正しく延長されます
- SPA ビューのキャッシュの初期化が正しく処理され、`viewsEnable` 設定に従うようになりました

## 2021年6月2日（PT）

### Adobe Target v2 拡張機能 0.14.2

- 最終的なバンドルに 2 つの `at.js` バージョン（オンデバイス判定があるものとないもの）が含まれるバグを修正しました。

## 2021年5月19日（PT）

### Adobe Target v2 拡張機能 0.14.1

- v0.14 リリースで導入された Target の読み込みアクションで、グローバル mbox 呼び出しが実行される問題を修正しました。

## 2021年5月14日（PT）

### Adobe Target v2 拡張機能 0.14

- [オンデバイス判定](./overview.md#load-target-with-on-device-decisioning)を使用した新しいアクション「ターゲット読み込み」が追加されました。このアクションは、オンデバイス判定機能を備えた `at.js` 2.5 を読み込みます。
- `at.js` を 2.5 に更新しました


## 2021年3月25日（PT）

### Adobe Target v2 拡張機能 0.13.7

- mbox リクエストに `targetPageParams` が含まれる問題を修正しました。 `targetPageParams` は `pageLoad` リクエストにのみ含めてください。
- グローバルオブジェクトの依存関係を直接参照で置き換えることで、タグ拡張機能のドキュメントおよびウィンドウのグローバルオブジェクトに関する問題を修正しました。
- `at.js` を 2.4.1 に更新しました。

## 2021年1月25日（PT）

### Adobe Target v2 拡張機能 0.13.6

- 統合プロファイル / プラットフォーム ID が API customerIds を配信する際のサポートを追加しました。
- 無効なスタイルタグが挿入される問題を修正しました。
- at.s から 2.4.0 への更新。
- 未定義のパラメーターが不正な配信リクエストを引き起こす可能性がある問題を解決しました。

## 2020年11月25日（PT）

### Adobe Target v2 拡張機能 0.13.4

- mbox パラメーターが UI に表示されないバグを修正しました。
- ブランディングのアップデート。
- `at.js` バージョンを 2.3.3 に更新しました。

## 2020年7月24日（PT）

### Adobe Target v2 拡張機能 0.13.3

- 非アクティブなアクティビティに対して QA モードのリンクが機能しない問題を修正しました。
- スクリプトまたはコードで `default` プロパティが `window` または `document` に追加されると拡張機能が機能しない問題を修正しました。

## 2020年6月15日（PT）

### Adobe Target v2 拡張機能 0.13.2

- CNAME とエッジの上書きを使用している際に、`at.js` 1.x で誤ってサーバードメインが作成され、Target リクエストが失敗することがあった問題を修正しました。
- Target および Adobe Analytics タグ拡張機能用の v2 タグ拡張機能を使用する際に、Target によって Analytics の sendBeacon 呼び出しで遅延が発生する問題を修正しました。
- `deviceIdLifetime` 設定を向上し、`targetGlobalSettings` 経由で上書きできるようにしました。

## 2020年3月25日（PT）

### Adobe Target v2 拡張機能 0.13.0

- `at.js` を v2.3 に更新しました。
- adobe.target.getOffer API に Target Global Mbox サポートを追加しました。
- パラメーターとページ読み込みパラメーターが正しく処理されない問題を修正しました。

## 2019年10月10日（PT）

### Adobe Target v2 拡張機能 0.12.0

- `at.js` を v2.2 に更新しました。
- Experience Cloud ID ライブラリ（ECID）v4.4 と `at.js` 2.2 間の統合のパフォーマンスを向上しました。
- 以前 ECID ライブラリは、`at.js` がエクスペリエンスを取得する前に、2 回のブロック呼び出しを行っていました。これを 1 回の呼び出しに短縮し、パフォーマンスが大幅に向上しました。

>[!NOTE]
>このパフォーマンス強化を利用するには、ECID タグ拡張機能を v4.4.1 にアップグレードしてください。

## 2019年7月31日（PT）

### Adobe Target v2 拡張機能 0.11.1

- 使用する拡張機能のバージョンを `at.js` 2.1.1 に更新しました
- パラメーターの処理に関する修正を追加しました。

## 2019年6月3日（PT）

### Adobe Target v2 拡張機能 0.11.0

- `at.js` 2.1 をサポートする新しいタグ拡張機能
