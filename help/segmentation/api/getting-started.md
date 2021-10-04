---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；api;
solution: Experience Platform
title: セグメント化サービス API の概要
topic-legacy: developer guide
description: 次のドキュメントは、Segmentation API を正しく操作するために知っておく必要がある追加情報を示しています。
exl-id: 41c0e50b-afed-45b8-85d7-a0c84ae090f5
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 50%

---

# セグメント化サービス API の概要 {#getting-started}

Adobe Experience Platform [!DNL Segmentation Service] では、セグメントを作成し、[!DNL Real-time Customer Profile] データからAdobe Experience Platformでオーディエンスを生成できます。

開発者ガイドでは、[!DNL Segmentation Service] の使用に関連する様々な [!DNL Experience Platform] サービスに関する十分な知識が必要です。

- [[!DNL Segmentation]](../home.md):データからオーディエンスセグメントを作成 [!DNL Real-time Customer Profile] できます。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Segmentation] API を正しく操作するために知っておく必要がある追加情報を示します。

## API 呼び出し例の読み取り

[!DNL Segmentation Service] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了している必要があります。[!DNL Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの操作について詳しくは、[ サンドボックスの概要に関するドキュメント ](../../sandboxes/home.md) を参照してください。

## 次の手順

[!DNL Segmentation Service] API を使用して呼び出しをおこなうには、左側のナビゲーションまたは [ 開発者ガイドの概要 ](./overview.md) 内で、使用可能なエンドポイントガイドの 1 つを選択します
