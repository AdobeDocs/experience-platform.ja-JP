---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Segmentation Service開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: aff81a4f3243ef77cbdfc776220a5de46e360084
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---


# Getting started with [!DNL Segmentation Service] {#getting-started}

Adobe Experience Platformセグメントサービスを使用すると、セグメントを作成し、 [!DNL Real-time Customer Profile] データをAdobe Experience Platformしてオーディエンスを生成できます。

開発者ガイドでは、の使用に関連する様々なExperience Platformサービスについて、作業を理解している必要があり [!DNL Segmentation Service]ます。

- [!DNL Segmentation](../home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [!DNL Experience Data Model (XDM) System](../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Segmentation] APIを正しく操作するために知っておく必要がある追加情報について説明します。

## サンプルAPI呼び出しの読み取り

APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が含まれています。 [!DNL Segmentation Service] 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

## 必須ヘッダー

また、APIドキュメントでは、Platformエンドポイントの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があります。 次に示すように、Experience PlatformAPI呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

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