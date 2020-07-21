---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Segmentation Service開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 1%

---


# Getting started with [!DNL Segmentation Service] {#getting-started}

Adobe Experience Platform [!DNL Segmentation Service] を使用すると、セグメントを作成し、 [!DNL Real-time Customer Profile] データからAdobe Experience Platformでオーディエンスを生成できます。

開発者ガイドでは、の使用に関連する様々な [!DNL Experience Platform] サービスについての作業的な理解が必要 [!DNL Segmentation Service]です。

- [!DNL Segmentation](../home.md): データからオーディエンスセグメントを作成でき [!DNL Real-time Customer Profile] ます。
- [!DNL Experience Data Model (XDM) System](../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Segmentation] APIを正しく操作するために知っておく必要がある追加情報について説明します。

## サンプルAPI呼び出しの読み取り

APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が含まれています。 [!DNL Segmentation Service] 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

## 必須ヘッダー

また、APIドキュメントでは、エンドポイントの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があり [!DNL Platform] ます。 次に示すように、認証チュートリアルで、 [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>でのサンドボックスの操作について詳し [!DNL Experience Platform]くは、 [サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

## 次の手順

APIを使用して呼び出しを行うには、左側のナビゲーションまたは [!DNL Segmentation Service][開発者ガイドの概要で、使用可能なエンドポイントガイドの1つを選択します](./overview.md)