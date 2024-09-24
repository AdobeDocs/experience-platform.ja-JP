---
title: Adobe Experience Platform クラウドコネクタ拡張機能のリリースノート
description: Adobe Experience Platform のクラウドコネクタ拡張機能に関する最新のリリースノート。
exl-id: 5ee85d9f-71f4-46ee-9064-4ceee1cf90e7
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: ht
source-wordcount: '128'
ht-degree: 100%

---

# Adobe Experience Platform クラウドコネクタ拡張機能リリースノート

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 2023年1月17日（PT）

v1.0.1

* 本文の Raw テキスト領域にペーストされた有効な JSON が、JSON ではなく文字列として保存される問題を修正しました。
* GET または HEAD リクエストで本文を設定することはできません。
* 5 kb を超える応答を保存すると、ルールの実行が失敗するバグを修正しました。
