---
title: Adobe Target v2 拡張機能のリリースノート
description: Adobe Experience Platform の Adobe Target v2 タグ拡張機能の最新のリリースノートです。
exl-id: c1a04e62-026d-4b16-aa70-bc6d5dbe6b2d
source-git-commit: 824fea41bc7e7082814648efd58184f5208e5e6f
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 75%

---

# Adobe Target v2 拡張機能リリースノート

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 2022 年 1 月 28 日（PT）

### Adobe Target v2 拡張機能 0.17.1

- をサポートするように更新しました `at.js` v2.8.1
- 固定 `pageLoad` マッピングされていません `target-global-mbox` ODD ハイブリッド実行モードの場合
- 次の分析の詳細に関する問題を修正しました： `mbox` リクエスト
- 開発依存関係をアップグレードしてセキュリティの脆弱性を修正

## 2022年1月7日（PT）

### Adobe Target v2 拡張機能 0.17.0

- をサポートするように更新しました `at.js` v2.8.0（機能使用状況およびパフォーマンステレメトリデータを収集）。  個人データは収集されません。この機能をオプトアウトするには、`targetGlobalSettings` で `telemetryEnabled` を `false` に設定します。

## 2021年10月28日（PT）

### Adobe Target v2 拡張機能 0.16.0

- をサポートするように更新しました `at.js` v2.7.0(Adobe Targetからダウンロード可能 )

## 2021 年 7 月 20 日（PT）

### Adobe Target v2 拡張機能 0.15.1

- `stringify` 関数名のクラッシュが原因で、`sessionId` や `requestId` などに対して誤った UUID 値が生成されていた問題を修正しました。

## 2021 年 7 月 16 日（PT）

### Adobe Target v2 拡張機能 0.15.0

- 常に cookie にセキュア属性を追加 `at.js` secureOnly 設定が true に設定されている
- `triggerView()` を使用する際にレスポンストークンを使用できるようになりました
- `CONTENT_RENDERING_NO_OFFERS` イベントに関連するバグを修正しました。Target からコンテンツが返されない場合でも、常に正しくトリガーされるようになりました
- プリフェッチリクエストを使用したときに、A4T のクリック指標の詳細が正しく返されるようになりました
- UUID の生成で `Math.random()` の使用を終了し、`window.crypto` に基づくようになりました
- `sessionId` cookie の有効期限は、すべてのネットワーク呼び出しで正しく延長されます
- SPA ビューのキャッシュの初期化が正しく処理され、`viewsEnable` 設定に従うようになりました

## 2021 年 6 月 2 日（PT）

### Adobe Target v2 拡張機能 0.14.2

- 最後のバンドルに 2 つが含まれるバグを修正しました `at.js` バージョン。1 つはオンデバイス判定を使用し、もう 1 つは非デバイス判定を使用します。

## 2021 年 5 月 19 日（PT）

### Adobe Target v2 拡張機能 0.14.1

- v0.14 リリースで導入された Target の読み込みアクションで、グローバル mbox 呼び出しが実行される問題を修正しました。

## 2021 年 5 月 14 日（PT）

### Adobe Target v2 拡張機能 0.14

- [On-Device Decisioning](./overview.md#load-target-with-on-device-decisioning) を使用した Load Target の新しいアクションが追加されました。このアクションは、On-Device Decisioning 機能を備えた 2.5 を読み込みます。`at.js`
- 更新済み `at.js` 2.5 へ


## 2021 年 3 月 25 日（PT）

### Adobe Target v2 拡張機能 0.13.7

- mbox リクエストに `targetPageParams` が含まれる問題を修正しました。 `targetPageParams` は `pageLoad` リクエストにのみ含めてください。
- グローバルオブジェクトの依存関係を直接参照で置き換えることで、タグ拡張機能のドキュメントおよびウィンドウのグローバルオブジェクトに関する問題を修正しました。
- 更新済み `at.js` を 2.4.1 に設定します。

## 2021 年 1 月 25 日（PT）

### Adobe Target v2 拡張機能 0.13.6

- 統合プロファイル / プラットフォーム ID が API customerIds を配信する際のサポートを追加しました。
- 無効なスタイルタグが挿入される問題を修正しました。
- at.s から 2.4.0 への更新。
- 未定義のパラメーターが不正な配信リクエストを引き起こす可能性がある問題を解決しました。

## 2020 年 11 月 25 日（PT）

### Adobe Target v2 拡張機能 0.13.4

- mbox パラメーターが UI に表示されないバグを修正しました。
- ブランディングのアップデート。
- 更新された `at.js` バージョン 2.3.3

## 2020 年 7 月 24 日（PT）

### Adobe Target v2 拡張機能 0.13.3

- 非アクティブなアクティビティに対して QA モードのリンクが機能しない問題を修正しました。
- スクリプトまたはコードで `default` プロパティが `window` または `document` に追加されると拡張機能が機能しない問題を修正しました。

## 2020 年 6 月 15 日（PT）

### Adobe Target v2 拡張機能 0.13.2

- CNAME とエッジの上書きを使用する際に、 `at.js` 1.x では、誤ってサーバードメインが作成され、Target リクエストが失敗する可能性があります
- Target および Adobe Analytics タグ拡張機能用の v2 タグ拡張機能を使用する際に、Target によって Analytics の sendBeacon 呼び出しで遅延が発生する問題を修正しました。
- `deviceIdLifetime` 設定を向上し、`targetGlobalSettings` 経由で上書きできるようにしました。

## 2020 年 3 月 25 日（PT）

### Adobe Target v2 拡張機能 0.13.0

- 更新済み `at.js` を v2.3 にアップロードします。
- adobe.target.getOffer API に Target Global Mbox サポートを追加しました。
- パラメーターとページ読み込みパラメーターが正しく処理されない問題を修正しました。

## 2019 年 10 月 10 日（PT）

### Adobe Target v2 拡張機能 0.12.0

- 更新済み `at.js` を v2.2 にアップロードします。
- Experience CloudID ライブラリ (ECID)v4.4 との統合のパフォーマンスが向上しました。 `at.js` 2.2.
- 以前は、ECID ライブラリが、以前に 2 回のブロック呼び出しをおこなっていました `at.js` エクスペリエンスを取得できませんでした。 これを 1 回の呼び出しに短縮し、パフォーマンスが大幅に向上しました。

>[!NOTE]
>このパフォーマンス強化を利用するには、ECID タグ拡張機能を v4.4.1 にアップグレードしてください。

## 2019 年 7 月 31 日（PT）

### Adobe Target v2 拡張機能 0.11.1

- 使用する拡張機能のバージョンを更新しました `at.js` 2.1.1
- パラメーターの処理に関する修正を追加しました。

## 2019 年 6 月 3 日（PT）

### Adobe Target v2 拡張機能 0.11.0

- をサポートする新しいタグ拡張 `at.js` 2.1
