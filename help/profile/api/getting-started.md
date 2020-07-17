---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPIの概要
topic: guide
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---


# APIの概要 [!DNL Real-time Customer Profile] {#getting-started}

APIを使用すると、計算済み属性の設定、エンティティへのアクセス、プロファイルデータの書き出し、不要なプロファイルセットやバッチの削除など、リソースに対する基本的なCRUD操作を実行できます。 [!DNL Real-time Customer Profile]

開発者ガイドを使用するには、データの操作に関係する様々なAdobe Experience Platformサービスについて、作業を理解しておく必要があり [!DNL Profile] ます。 APIの使用を開始する前に、次のサービスのドキュメントを確認して [!DNL Real-time Customer Profile] ください。

* [!DNL Real-time Customer Profile](../home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
* [!DNL Adobe Experience Platform Identity Service](../../identity-service/home.md): デバイスとシステム間でIDをブリッジ化することで、顧客と行動をより良く表示できます。
* [!DNL Adobe Experience Platform Segmentation Service](../../segmentation/home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
* [!DNL Experience Data Model (XDM)](../../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [!DNL Sandboxes](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Profile] APIエンドポイントの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

## サンプルAPI呼び出しの読み取り

APIドキュメントには、リクエストを正しくフォーマットする方法を示すAPI呼び出しの例が含まれています。 [!DNL Real-time Customer Profile] 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

## 必須ヘッダー

また、APIドキュメントでは、エンドポイントの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があり [!DNL Platform] ます。 次に示すように、認証チュートリアルで、 [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへの要求には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

リクエスト本文のペイロードを含むすべてのリクエスト（POST、PUT、PATCH呼び出しなど）には、 `Content-Type` ヘッダーが必要です。 各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに提供されます。

## 次の手順

APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。 [!DNL Real-time Customer Profile]