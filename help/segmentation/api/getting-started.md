---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；Segmentation Service;api;
solution: Experience Platform
title: Segmentation Service API の概要
description: 次のドキュメントでは、Segmentation API を正しく使用するために知っておく必要がある追加情報を示します。
role: Developer
exl-id: 41c0e50b-afed-45b8-85d7-a0c84ae090f5
source-git-commit: f6d700087241fb3a467934ae8e64d04f5c1d98fa
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 62%

---

# Segmentation Service API の概要 {#getting-started}

Adobe Experience Platform [!DNL Segmentation Service] を使用すると、Adobe Experience Platformのセグメント定義またはその他のソースを使用して、[!DNL Real-Time Customer Profile] データからオーディエンスを作成できます。

デベロッパーガイドには、[!DNL Segmentation Service] の使用に関連する様々な [!DNL Experience Platform] サービスについての十分な知識が必要です。

- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):[!DNL Real-Time Customer Profile] データからオーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。セグメント化を最大限に活用するには、[データモデリングのベストプラクティス](../../xdm/schema/best-practices.md)に従って、データがプロファイルとイベントとして取り込まれていることを確認してください。
- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、[!DNL Segmentation] API を正しく使用するために必要な追加情報を示します。

## API 呼び出し例の読み取り

[!DNL Segmentation Service] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。[!DNL Experience Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの使用について詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## 次の手順

[!DNL Segmentation Service] API を使用して呼び出すには、左側のナビゲーションを使用して、または [ 開発者ガイドの概要内で、使用可能なエンドポイントガイドの 1 つを選択 ](./overview.md) ます。
