---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API；サンドボックス；サンドボックス；サンドボックス
solution: Experience Platform
title: サンドボックスAPIガイドの付録
description: このドキュメントでは、サンドボックスAPIの操作に関する補足情報を提供します。
topic-legacy: developer guide
source-git-commit: e4067f79e9da106fe2d2a86fa0024c2e5fe5d0ba
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 10%

---

# サンドボックスAPIガイドの付録

このドキュメントでは、[!DNL Sandbox] APIの使用に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[[!DNL Sandbox] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sandbox-api.yaml)では、サンドボックスのリスト表示時にクエリパラメーターを使用して、結果をページ化およびフィルタリングできます。

>[!NOTE]
>
>`limit`と`offset`のクエリパラメーターは、一緒に指定する必要があります。 1つのみを指定した場合、APIはエラーを返します。 noneを指定した場合、デフォルトの制限は50で、オフセットは0です。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 |
| `offset` | 応答リストを開始する（オフセット）最初のレコードのエンティティ数。 |
