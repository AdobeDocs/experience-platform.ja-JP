---
keywords: Experience Platform;home;popular topics;catalog service;catalog;Catalog service;Catalog
solution: Experience Platform
title: カタログサービス開発者ガイド
topic: developer guide
description: この開発者ガイドでは、カタログ API の使用を始める手順を説明します。次に、カタログを使用して主要な操作を実行するためのサンプル API 呼び出しを提供します。
translation-type: tm+mt
source-git-commit: c8e53a25c5b22e8d840658fe26ff47875947a6d0
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 45%

---


# [!DNL Catalog Service] 開発ガイド

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの位置と系統に関する記録システムです。 [!DNL Catalog] は、データ自体にアクセスすることなく、データに関する情報を検索できるメタデータストア（「カタログ」） [!DNL Experience Platform]として機能します。 See the [[!DNL Catalog] overview](../home.md) for more information.

This developer guide provides steps to help you start using the [!DNL Catalog] API. The guide then provides sample API calls for performing key operations using [!DNL Catalog].

## 前提条件

[!DNL Catalog] では、様々な種類のリソースおよび操作のメタデータを追跡 [!DNL Experience Platform]します。 This developer guide requires a working understanding of the various [!DNL Experience Platform] services involved with creating and managing these resources:

* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。
* [バッチ取得](../../ingestion/batch-ingestion/overview.md)[!DNL Experience Platform]： が CSV や Parket などのデータファイルからデータを取得して保存する方法。
* [ストリーミング取得](../../ingestion/streaming-ingestion/overview.md)[!DNL Experience Platform]： がクライアントサイドおよびサーバーサイドのデバイスからデータをリアルタイムで取得し、保存する方法。

The following sections provide additional information that you will need to know or have on-hand in order to successfully make calls to the [!DNL Catalog Service] API.

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type: application/json

## Best practices for [!DNL Catalog] API calls

When performing GET requests to the [!DNL Catalog] API, best practice is to include query parameters in your requests in order to return only the objects and properties that you need. フィルターを適用しないリクエストの応答ペイロードのサイズは 3 GB に達っすることがあり、全体的なパフォーマンスが低下する可能性があります。

特定のオブジェクトを表示するには、リクエストパスに ID を含めるか、または `properties` や `limit` などのクエリーパラメーターを使用して応答をフィルターします。フィルターは、ヘッダーおよびクエリーパラメーターとして渡すことができ、クエリーパラメーターとして渡されたフィルターが優先されます。詳しくは、[カタログデータのフィルター](filter-data.md)に関するドキュメントを参照してください。

Since some queries can put a heavy load on the API, global limits have been implemented on [!DNL Catalog] queries to further support best practices.

## 次の手順

This document covered the prerequisite knowledge required to make calls to the [!DNL Catalog] API. 次に、この開発者ガイドに記載されているサンプル呼び出しに進み、その手順に従うことができます。

Most of the examples in this guide use the `/dataSets` endpoint, but the principles can be applied to other endpoints within [!DNL Catalog] (such as `/batches` and `/accounts`). 各エンドポイントで使用できるすべての呼び出しと操作の完全なリストについては、『[カタログサービス API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)』を参照してください。

For a step-by-step workflow that demonstrates how the [!DNL Catalog] API is involved with data ingestion, see the tutorial on [creating a dataset](../datasets/create.md).
