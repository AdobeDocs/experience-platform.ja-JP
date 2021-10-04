---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API；サンドボックス；サンドボックス；サンドボックス
solution: Experience Platform
title: サンドボックス API ガイドの付録
description: このドキュメントでは、サンドボックス API の操作に関する補足情報を提供します。
topic-legacy: developer guide
exl-id: 48ffea01-f1b4-48c6-a6f5-c321074023d3
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 4%

---

# サンドボックス API ガイドの付録

このドキュメントでは、[!DNL Sandbox] API の操作に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox) では、サンドボックスのリスト表示時にクエリパラメーターを使用して、結果をページ化およびフィルタリングできます。

>[!NOTE]
>
>`limit` と `offset` のクエリーパラメーターは、一緒に指定する必要があります。 1 つのみを指定した場合、API はエラーを返します。 none を指定した場合、デフォルトの制限は 50 で、オフセットは 0 です。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 |
| `offset` | 応答リストを開始（オフセット）する最初のレコードのエンティティ数。 |
