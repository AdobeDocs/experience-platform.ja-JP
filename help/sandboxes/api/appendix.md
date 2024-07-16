---
keywords: Experience Platform；ホーム；人気のトピック；api;API；サンドボックス；サンドボックス；サンドボックス
solution: Experience Platform
title: サンドボックス API ガイドの付録
description: このドキュメントでは、Sandbox API の操作に関する補足情報を提供します。
role: Developer
exl-id: 48ffea01-f1b4-48c6-a6f5-c321074023d3
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 4%

---

# サンドボックス API ガイドの付録

このドキュメントでは、[!DNL Sandbox] API の操作に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox) では、サンドボックスをリストする際に、クエリパラメーターを使用して結果をページ化およびフィルタリングできます。

>[!NOTE]
>
>`limit` クエリパラメーターと `offset` クエリパラメーターは一緒に指定する必要があります。 1 つのみを指定すると、API はエラーを返します。 何も指定しない場合、デフォルトの制限値は 50、オフセットは 0 です。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 |
| `offset` | 応答リストを開始（オフセット）する、最初のレコードからのエンティティ数。 |
