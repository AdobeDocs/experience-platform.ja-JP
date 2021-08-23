---
title: Adobe Target v2 拡張機能のリリースノート
description: Adobe Experience Platform の Adobe Target v2 タグ拡張機能の最新リリースノートです。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 76%

---

# Adobe Target v2 拡張機能のリリースノート

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 2021 年 7 月 20 日（PT）

### Adobe Target v2 Extension 0.15.1

- `stringify`関数名のクラッシュが原因で、`sessionId`や`requestId`に対して誤ったUUID値が生成されていた問題を修正しました。

## 2021 年 7 月 16 日（PT）

### Adobe Target v2 Extension 0.15.0

- at.js設定secureOnlyがtrueに設定されている場合は常に、セキュア属性をcookieに追加します。
- `triggerView()`
- `CONTENT_RENDERING_NO_OFFERS`イベントに関連するバグを修正しました。 現在は、Targetからコンテンツが返されない場合は常に正しくトリガーされます
- プリフェッチ要求を使用すると、A4Tクリック指標の詳細が正しく返される
- UUID生成は、`Math.random()`を使用しなくなりましたが、`window.crypto`を使用します
- `sessionId` すべてのネットワーク呼び出しでcookieの有効期限が正しく拡張される
- SPAビューキャッシュの初期化が正しく処理され、`viewsEnable`設定に従うようになりました。

## 2021 年 6 月 2 日（PT）

### Adobe Target v2 Extension 0.14.2

- 最終的なバンドルに2つのat.jsバージョン（1つはオンデバイス判定付き、もう1つは非デバイス）が含まれるバグを修正します。

## 2021 年 5 月 19 日（PT）

### Adobe Target v2 Extension 0.14.1

- v0.14 リリースで導入されたターゲットの読み込みアクションで、グローバル mbox 呼び出しが実行される問題を修正しました。

## 2021 年 5 月 14 日（PT）

### Adobe Target v2 Extension 0.14

- [On-Device Decisioning](./overview.md#load-target-with-on-device-decisioning) を使用した Load Target の新しいアクションが追加されました。このアクションは、On-Device Decisioning 機能を備えた at.js 2.5 を読み込みます。
- at.js を 2.5 にアップデートしました。


## 2021 年 3 月 25 日（PT）

### Adobe Target v2 Extension 0.13.7

- mbox リクエストに `targetPageParams` が含まれる問題を修正しました。 `targetPageParams` は `pageLoad` リクエストにのみ含めてください。
- グローバルオブジェクトの依存関係を直接参照に置き換えることで、タグ拡張機能のドキュメントおよびウィンドウのグローバルオブジェクトに関する問題を修正しました。
- at.js を 2.4.1 に更新しました。

## 2021 年 1 月 25 日（PT）

### Adobe Target v2 Extension 0.13.6

- 統合プロファイル / プラットフォーム ID が API customerIds を配信する際のサポートを追加しました。
- 無効なスタイルタグが挿入される問題を修正しました。
- at.s から 2.4.0 への更新
- 未定義のパラメーターが不正な配信リクエストを引き起こす可能性がある問題を解決しました。

## 2020 年 11 月 25 日（PT）

### Adobe Target v2 Extension 0.13.4

- mbox パラメーターが UI に表示されないバグを修正しました。
- ブランディングのアップデート。
- at.js バージョンを 2.3.3 にアップデートしました。

## 2020 年 7 月 24 日（PT）

### Adobe Target v2 Extension 0.13.3

- 非アクティブなアクティビティに対して QA モードのリンクが機能しない問題を修正しました。
- スクリプトまたはコードで `default` プロパティが `window` または `document` に追加されると拡張機能が機能しない問題を修正しました。

## 2020 年 6 月 15 日（PT）

### Adobe Target v2 Extension 0.13.2

- CNAME とエッジの上書きを使用している際に、at.js 1.x で誤ってサーバードメインが作成され、Target リクエストが失敗することがあった問題を修正しました。
- Target および Adobe Analytics タグ拡張機能用の v2 タグ拡張機能を使用する際に、Target によって Analytics の sendBeacon 呼び出しで遅延が発生する問題を修正しました
- `deviceIdLifetime` 設定を向上し、`targetGlobalSettings` 経由で上書きできるようにしました。

## 2020 年 3 月 25 日（PT）

### Adobe Target v2 Extension 0.13.0

- at.js を v2.3 に更新しました。
- adobe.target.getOffer API に Target Global Mbox サポートを追加しました。
- パラメーターとページ読み込みパラメーターが正しく処理されない問題を修正しました。

## 2019 年 10 月 10 日（PT）

### Adobe Target v2 Extension 0.12.0

- at.js を v2.2 に更新しました。
- Experience Cloud ID ライブラリ（ECID）v4.4 と at.js 2.2 間の統合のパフォーマンスを向上しました。
- 以前、ECID ライブラリは、at.js がエクスペリエンスを取得する前に、2 回のブロック呼び出しをおこなっていました。これを 1 回の呼び出しに短縮し、パフォーマンスが大幅に向上しました。

>[!NOTE]
>このパフォーマンス強化を利用するには、ECID タグ拡張機能を v4.4.1 にアップグレードしてください。

## 2019 年 7 月 31 日（PT）

### Adobe Target v2 Extension 0.11.1

- 使用する拡張機能のバージョンを at.js 2.1.1 に更新しました。
- パラメーターの処理に関する修正を追加しました。

## 2019 年 6 月 3 日（PT）

### Adobe Target v2 Extension 0.11.0

- at.js 2.1 をサポートする新しいタグ拡張機能
