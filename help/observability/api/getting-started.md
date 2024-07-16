---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
solution: Experience Platform
title: Observability Insights API の概要
description: Observability Insights API を使用すると、様々なAdobe Experience Platform機能の指標データを取得できます。 このドキュメントでは、Observability Insights API を呼び出す前に知っておく必要があるコア概念の概要を説明します。
exl-id: 3b120bd6-155d-467e-b98e-05478f8a4cc5
source-git-commit: 5a14eb5938236fa7186d1a27f28cee15fe6558f6
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 51%

---

# [!DNL Observability Insights] API の概要

[!DNL Observability Insights] API を使用すると、様々なAdobe Experience Platform機能の指標データを取得できます。 このドキュメントでは、[!DNL Observability Insights] API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## API 呼び出し例の読み取り

[!DNL Observability Insights] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、[Experience Platformトラブルシューテングガイド ](../../landing/troubleshooting.md) の API 呼び出し例の読み方に関する節を参照してください。

## 必須ヘッダー

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。 [!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

* `x-sandbox-name: {SANDBOX_NAME}`

## 次の手順

[!DNL Observability Insights] API を使用した呼び出しを開始するには、[ 指標エンドポイントガイド ](./metrics.md) に進みます。
