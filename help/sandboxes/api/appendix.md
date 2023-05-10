---
keywords: Experience Platform；ホーム；人気のトピック；API;API；サンドボックス；サンドボックス；サンドボックス
solution: Experience Platform
title: サンドボックス API ガイドの付録
description: このドキュメントでは、サンドボックス API の操作に関する補足情報を提供します。
exl-id: 48ffea01-f1b4-48c6-a6f5-c321074023d3
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 8%

---

# サンドボックス API ガイドの付録

このドキュメントでは、 [!DNL Sandbox] API

## クエリパラメーターの使用 {#query}

この [[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox) では、サンドボックスをリストする際のページへのクエリパラメーターの使用、および結果のフィルタリングをサポートしています。

>[!NOTE]
>
>この `limit` および `offset` クエリパラメーターは、一緒に指定する必要があります。 1 つのみを指定した場合、API はエラーを返します。 none を指定した場合、デフォルトの制限は 50 で、オフセットは 0 です。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 |
| `offset` | 応答リストを開始する（オフセットする）最初のレコードのエンティティの数。 |
