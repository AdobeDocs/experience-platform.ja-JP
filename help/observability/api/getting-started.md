---
keywords: Experience Platform；ホーム；人気のあるトピック；日付範囲
solution: Experience Platform
title: 観察性インサイトAPI使用の手引き
topic-legacy: developer guide
description: Observibility Insights APIを使用すると、Adobe Experience Platformの様々な機能の指標データを取得できます。 このドキュメントでは、Observibility Insights APIを呼び出す前に知っておく必要があるコア概念の紹介を行います。
exl-id: 3b120bd6-155d-467e-b98e-05478f8a4cc5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 24%

---

# [!DNL Observability Insights] API使用の手引き

[!DNL Observability Insights] APIを使用すると、Adobe Experience Platformの様々な機能の指標データを取得できます。 このドキュメントでは、[!DNL Observability Insights] APIを呼び出す前に知っておく必要があるコア概念の紹介を行います。

## API 呼び出し例の読み取り

[!DNL Observability Insights] APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が記載されています。 この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプルAPI呼び出しに関するドキュメントで使用される規則について詳しくは、[Experience Platformトラブルシューティングガイド](../../landing/troubleshooting.md)のAPI呼び出し例を読む方法に関する節を参照してください。

## 必須ヘッダー

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。 [!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

* `x-sandbox-name: {SANDBOX_NAME}`

## 次の手順

[!DNL Observability Insights] APIを使用して呼び出しを開始するには、[metrics endpoint guide](./metrics.md)に進みます。
