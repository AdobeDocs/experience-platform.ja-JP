---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;Segmentation Service;api;
solution: Experience Platform
title: セグメント化サービス開発者ガイド
topic: developer guide
description: セグメントAPIを正しく機能させるために必要な追加情報については、次のドキュメントを参照してください。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 26%

---


# Getting started with [!DNL Segmentation Service] {#getting-started}

Adobe Experience Platform [!DNL Segmentation Service] では、セグメントを作成し、データからAdobe Experience Platformでオーディエンスを生成でき [!DNL Real-time Customer Profile] ます。

開発者ガイドでは、の使用に関連する様々な [!DNL Experience Platform] サービスについての作業的な理解が必要 [!DNL Segmentation Service]です。

- [[!DNL Segmentation]](../home.md):データからオーディエンスセグメントを作成でき [!DNL Real-time Customer Profile] ます。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully work with the [!DNL Segmentation] API.

## API 呼び出し例の読み取り

The [!DNL Segmentation Service] API documentation provides example API calls to demonstrate how to format your requests. この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](../../tutorials/authentication.md)を完了している必要があります。[!DNL Platform]Completing the authentication tutorial provides the values for each of the required headers in [!DNL Experience Platform] API calls, as shown below:

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox in which the operation will take place:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on working with sandboxes in [!DNL Experience Platform], see the [sandboxes overview documentation](../../sandboxes/home.md).

## 次の手順

APIを使用して呼び出しを行うには、左側のナビゲーションまたは [!DNL Segmentation Service][開発者ガイドの概要で、使用可能なエンドポイントガイドの1つを選択します](./overview.md)