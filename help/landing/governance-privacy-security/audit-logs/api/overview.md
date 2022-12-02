---
title: 監査クエリ API ガイド
description: 監査クエリは、開発者がAdobe Experience Platformで何アクションを実行したかを確認できる RESTful API です。
source-git-commit: 5b3459711f41430977f9d7b06f8b35801739207c
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 9%

---

# [!DNL Audit Query] API ガイド

この [!DNL Audit Query] API は、様々なAdobe Experience Platform機能のイベントデータをプログラムで取得および監視できるエンドポイントを提供します。 エンドポイントの概要を以下に示します。 次を参照してください： [入門ガイド](./getting-started.md) 必要なヘッダー、サンプル API 呼び出しなどに関する重要な情報については、を参照してください。

使用可能なすべてのエンドポイントと CRUD 操作を表示するには、[[!DNL Audit Query] API swagger](https://www.adobe.io/experience-platform-apis/references/audit-query/) を参照してください。

## イベント

監査イベントは、アクションタイプ、日時、アクションを実行したユーザーの E メール ID、Adobe Experience Platformの様々な機能のアクションタイプに関連する追加の属性など、Platform のユーザーアクションに関するインサイトを提供します。 API を使用して指標を取得する方法については、 [イベントエンドポイントガイド](./events.md).

## エクスポート

監査書き出しを使用すると、ペイロードで取得するイベントを指定して、イベントデータを取得できます。 API を使用して指標を取得する方法については、 [書き出しエンドポイントガイド](./export.md).
