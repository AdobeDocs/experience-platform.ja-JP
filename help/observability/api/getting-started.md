---
keywords: Experience Platform;home;popular topics;date range
solution: Experience Platform
title: Observibility Insights API使用の手引き
topic: developer guide
description: Observibility Insights APIを使用すると、Adobe Experience Platformの様々な機能の指標データを取得できます。 このドキュメントでは、Observibility Insights APIを呼び出す前に知っておく必要があるコア概念の紹介を行います。
translation-type: tm+mt
source-git-commit: c5455dc0812b251483170ac19506d7c60ad4ecaa
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 11%

---


# Getting started with the [!DNL Observability Insights] API

この [!DNL Observability Insights] APIを使用すると、Adobe Experience Platformの様々な機能の指標データを取得できます。 This document provides an introduction to the core concepts you need to know before attempting to make calls to the [!DNL Observability Insights] API.

## API 呼び出し例の読み取り

The [!DNL Observability Insights] API documentation provides example API calls to demonstrate how to format your requests. この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。For information on the conventions used in documentation for sample API calls, see the section on how to read example API calls in the [Experience Platform troubleshooting guide](../../landing/troubleshooting.md).

## 必須ヘッダー

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in. For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

* `x-sandbox-name: {SANDBOX_NAME}`

## 次の手順

APIを使用した呼び出しを開始するには、 [!DNL Observability Insights][指標エンドポイントガイドに進みます](./metrics.md)。