---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；api;
solution: Experience Platform
title: セグメント化サービス開発者ガイド
topic: developer guide
description: セグメントAPIを正しく機能させるために必要な追加情報については、次のドキュメントを参照してください。
translation-type: tm+mt
source-git-commit: ece2ae1eea8426813a95c18096c1b428acfd1a71
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 25%

---


# [!DNL Segmentation Service] {#getting-started}はじめに

Adobe Experience Platform[!DNL Segmentation Service]では、セグメントを作成し、[!DNL Real-time Customer Profile]データからAdobe Experience Platformでオーディエンスを生成できます。

開発者ガイドでは、[!DNL Segmentation Service]の使用に関連する様々な[!DNL Experience Platform]サービスに関する作業的な理解が必要です。

- [[!DNL Segmentation]](../home.md):デー [!DNL Real-time Customer Profile] タからオーディエンスセグメントを作成できます。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、[!DNL Segmentation] APIを正しく動作させるために知っておく必要のある追加情報について説明します。

## API 呼び出し例の読み取り

[!DNL Segmentation Service] APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が記載されています。 この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了している必要があります。[!DNL Platform]次に示すように、認証チュートリアルで、[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定します。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform]でのサンドボックスの操作について詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

## 次の手順

[!DNL Segmentation Service] APIを使用して呼び出すには、左側のナビゲーションまたは[開発者ガイドの概要](./overview.md)内で、使用可能なエンドポイントガイドの1つを選択します