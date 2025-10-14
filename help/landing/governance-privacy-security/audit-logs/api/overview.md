---
title: 監査クエリ API ガイド
description: 監査クエリは、開発者がAdobe Experience Platformで誰が何のアクションを実行したかを確認できる RESTful API です。
role: Developer
feature: Audits, API
exl-id: 9ed291c6-ff8b-4d9b-9fed-d1e3fa8f92fb
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 9%

---

# [!DNL Audit Query] API ガイド

[!DNL Audit Query] API は、様々なAdobe Experience Platform機能のイベントデータをプログラムで取得および監視できるエンドポイントを提供します。 エンドポイントの概要を以下に示します。 必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報については、[&#x200B; はじめる前に &#x200B;](./getting-started.md) を参照してください。

使用可能なすべてのエンドポイントと CRUD 操作を表示するには、[[!DNL Audit Query] API swagger](https://www.adobe.io/experience-platform-apis/references/audit-query/) を参照してください。

## イベント

監査イベントは、アクションタイプ、日時、アクションを実行したユーザーの電子メール ID、Experience Platformの様々な機能のアクションタイプに関連する追加の属性など、Adobe Experience Platformでのユーザーのアクションに関するインサイトを提供します。 API を使用して指標を取得する方法については、[&#x200B; イベントエンドポイントガイド &#x200B;](./events.md) を参照してください。

## 書き出し

監査エクスポートを使用すると、取得するイベントをペイロードに指定してイベントデータを取得できます。 API を使用して指標を取得する方法については、[&#x200B; エクスポートエンドポイントガイド &#x200B;](./export.md) を参照してください。
