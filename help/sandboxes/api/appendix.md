---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API；サンドボックス；サンドボックス；サンドボックス
solution: Experience Platform
title: サンドボックスAPIガイドの付録
description: このドキュメントでは、サンドボックスAPIの操作に関する補足情報を提供します。
topic-legacy: developer guide
source-git-commit: f5ce7b7f09c624c53065757bb8a9b09f989dce0a
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 4%

---

# サンドボックスAPIガイドの付録

このドキュメントでは、[!DNL Sandbox] APIの使用に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox)では、サンドボックスのリスト表示時にクエリパラメーターを使用して、結果をページ化およびフィルタリングできます。

>[!NOTE]
>
>`limit`と`offset`のクエリパラメーターは、一緒に指定する必要があります。 1つのみを指定した場合、APIはエラーを返します。 noneを指定した場合、デフォルトの制限は50で、オフセットは0です。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 |
| `offset` | 応答リストを開始する（オフセット）最初のレコードのエンティティ数。 |
