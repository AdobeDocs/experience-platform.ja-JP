---
keywords: Experience Platform;home;popular topics;query service;Query service;query
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: query templates
description: この開発者ガイドでは、Adobe Experience Platform のクエリサービス API で様々な操作を実行する手順を説明します。
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 38%

---


# [!DNL Query Service] 開発ガイド

This developer guide provides steps for performing various operations in the Adobe Experience Platform [!DNL Query Service] API.

## はじめに

This guide requires a working understanding of the various Adobe Experience Platform services involved with using [!DNL Query Service].

- [[!DNL Query Service]](../home.md):データセットをクエリし、結果のクエリをの新しいデータセットとして取り込む機能を提供 [!DNL Experience Platform]します。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
- [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully use [!DNL Query Service] using the API.

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。このドキュメントでサンプル API 呼び出しに使用される表記規則について詳しくは、 トラブルシューティングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Experience Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Platform] API calls, as shown below:

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox in which the operation will take place:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on working with sandboxes in [!DNL Experience Platform], see the [sandboxes overview documentation](../../sandboxes/home.md).

## サンプル API 呼び出し

Now that you understand what headers to use, you are ready to begin making calls to the [!DNL Query Service] API. The following documents walk through the various API calls you can make using the [!DNL Query Service] API. 各サンプル呼び出しには、一般的な API 形式、必要なヘッダーを示すサンプルリクエスト、サンプルの応答が含まれています。

- [クエリ](queries.md)
- [接続パラメータ](connection-parameters.md)
- [スケジュールクエリ](scheduled-queries.md)
- [スケジュールクエリの実行](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)

## 次の手順

Now that you have learned how to make calls using the [!DNL Query Service] API, you can create your own non-interactive queries. クエリの作成方法について詳しくは、[SQL リファレンスガイド](../sql/overview.md)を参照してください。