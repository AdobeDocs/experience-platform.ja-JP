---
title: Adobe Target 拡張機能のリリースノート
description: Adobe Experience Platform の Adobe Target タグ拡張機能に関する最新のリリースノートです。
exl-id: ba29f614-c3cd-4e0b-b043-2b1c17567def
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 100%

---

# Adobe Target リリースノート

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 2020 年 7 月 24 日（PT）

### Adobe Target Extension 0.11.3

* スクリプトまたはコードで `default` プロパティが `window` または `document` に追加されると拡張機能が機能しない問題を修正しました。

## 2020 年 6 月 15 日（PT）

### Adobe Target Extension 0.11.2

* CNAME とエッジの上書きを使用している際に、at.js 1.x で誤ってサーバードメインが作成され、Target リクエストが失敗することがあった問題を修正しました。

## 2020 年 3 月 25 日（PT）

### Adobe Target Extension 0.11.1

* at.js を v1.8.1 に更新しました。
* パラメーターとページ読み込みパラメーターが正しく処理されない問題を修正しました。

## 2019 年 10 月 10 日（PT）

### Adobe Target Extension 0.11.0

* at.js を v1.8 に更新しました。
* Experience Cloud ID ライブラリ（ECID）v4.4 と at.js 1.8 間の統合のパフォーマンスを向上しました。
* 以前、ECID ライブラリは、at.js がエクスペリエンスを取得する前に、2 回のブロック呼び出しをおこなっていました。これを 1 回の呼び出しに短縮し、パフォーマンスが大幅に向上しました。

>[!NOTE]
>このパフォーマンス強化を利用するには、Adobe Experience Platform の ECID タグ拡張機能を v4.4.1 にアップグレードしてください。

## 2019 年 7 月 31 日（PT）

### Adobe Target Extension 0.10.1

* Adobe Target のタグ拡張機能を処理するパラメーターのホットフィックス

## 2019 年 5 月 4 日（PT）

### Adobe Target Extension 0.10.0

* 最新の Google Chrome 変更によるデータ要素の問題を修正しました。

## 2019 年 3 月 14 日（PT）

### Adobe Target Extension 0.9.3

* 使用する拡張機能のバージョンを at.js 1.7.1 に更新しました。

## 2019 年 2 月 20 日（PT）

### Adobe Target Extension 0.9.2

* Target と Analytics の拡張機能の競合状態を修正しました。

## 2019 年 2 月 12 日（PT）

### Adobe Target Extension 0.9.1

#### **機能**

* Target タグを実行する方法とタイミングを制御するために、タグを介してサポートされるオプトインプライバシー機能がサポートされている at.js 1.7.0 を使用するように拡張機能を更新しました。オプトインの実装をセットアップする方法については、「タグドキュメント」を確認してください。空の値を持つ mbox パラメーターが Target に送信されるかどうかをカスタマイズできます。

## 2019 年 1 月 23 日（PT）

### Adobe Target Extension 0.8.4

* at.js をバージョン 1.6.4 に更新しました。
* 拡張機能の UI を Adobe Spectrum に移行しました。

## 2018 年 11 月 15 日（PT）

### Adobe Target Extension 0.8.2

* at.js をバージョン 1.6.3 に更新しました。

## 2018 年 10 月 24 日（PT）

### Adobe Target Extension 0.8.1

* at.js をバージョン 1.6.2 に更新しました。

## 2018 年 8 月 23 日（PT）

### Adobe Target Extension 0.8.0

* at.js をバージョン 1.6.0 に更新しました。

## 2018 年 8 月 10 日（PT）

### Adobe Target Extension 0.7.2

* マイナーな変更
* `extension.json` ファイルの `exchangeUrl` プロパティを更新しました。

## 2018 年 8 月 1 日（PT）

### Adobe Target Extension 0.7.1

* 小規模な修正。

## 2018 年 6 月 18 日（PT）

### Adobe Target Extension 0.7.0

* at.js バージョンを 1.5.0 に更新しました。
* Media Manager が IE11 で null 参照エラーをスローする問題を修正しました。

## 2018 年 6 月 15 日（PT）

### Adobe Target Extension 0.6.0

#### **機能**

* Target 拡張機能は at.js v1.3.1 を使用するよう更新されました。Analytics を使用して Target をデプロイした場合、Target 呼び出し（リダイレクトオファーを含む）が解決されるまで待ってから Analytics を実行するようになりました。これにより、過去に存在していた競合条件を解決します。

## 2018 年 2 月 22 日（PT）

### Adobe Target Extension 0.4.1

#### **機能**

* extension.json をリストする Adobe Exchange を追加しました。
* Target が無効かどうか、およびオーサリングが有効かどうかを確認するチェックを追加しました。

#### **バグの修正**

* タグを通じてデプロイしたときに、Visual Experience Composer がページの非表示を解除できなかった Adobe Target 拡張機能のエラーを修正しました。

## 2018 年 2 月 8 日（PT）

### Adobe Target Extension 0.4.0

#### **機能**

* 拡機能設定画面のビューを更新しました。
* at.js はバージョン 1.2.3 に更新されました（JSON オファーのサポートを追加しました）。
