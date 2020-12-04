---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
title: リアルタイム顧客プロファイルAPIの概要
topic: guide
translation-type: tm+mt
source-git-commit: 59cf089a8bf7ce44e7a08b0bb1d4562f5d5104db
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 29%

---


# Getting started with the [!DNL Real-time Customer Profile] API {#getting-started}

Using the [!DNL Real-time Customer Profile] API, you can perform basic CRUD operations against Profile resources, such as configuring computed attributes, accessing entities, exporting Profile data, and deleting unneeded datasets or batches.

Using the developer guide requires a working understanding of the various Adobe Experience Platform services involved in working with [!DNL Profile] data. Before beginning to work with the [!DNL Real-time Customer Profile] API, please review the documentation for the following services:

* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
* [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md):デバイスとシステム間でIDをブリッジ化することで、顧客と行動をより良く表示できます。
* [[!DNL Adobe Experience Platform Segmentation Service]](../../segmentation/home.md):リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully make calls to [!DNL Profile] API endpoints.

## API 呼び出し例の読み取り

The [!DNL Real-time Customer Profile] API documentation provides example API calls to demonstrate how to properly format requests. この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](../../tutorials/authentication.md)を完了している必要があります。[!DNL Platform]Completing the authentication tutorial provides the values for each of the required headers in [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. Requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに指定されます。

## 次の手順

APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。 [!DNL Real-time Customer Profile]