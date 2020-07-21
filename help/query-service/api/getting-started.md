---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: query templates
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---


# [!DNL Query Service] 開発ガイド

この開発者ガイドでは、Adobe Experience Platform [!DNL Query Service] APIの様々な操作を実行する手順を説明します。

## はじめに

このガイドでは、の使用に関連する様々なAdobe Experience Platformサービスについて、十分に理解している必要があり [!DNL Query Service]ます。

- [!DNL Query Service](../home.md): データセットをクエリし、結果のクエリをの新しいデータセットとして取り込む機能を提供 [!DNL Experience Platform]します。
- [!DNL Experience Data Model (XDM) System](../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
- [!DNL Sandboxes](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、APIを正しく使用するために知っておく必要がある追加情報につ [!DNL Query Service] いて説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 このドキュメントでサンプルAPI呼び出しに使用される表記について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Experience Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>でのサンドボックスの操作について詳し [!DNL Experience Platform]くは、 [サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

## サンプルAPI呼び出し

これで、使用するヘッダーが分かったので、 [!DNL Query Service] APIの呼び出しを開始する準備が整いました。 以下のドキュメントでは、 [!DNL Query Service] APIを使用して実行できる様々なAPI呼び出しについて説明します。 各サンプル呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

- [クエリ](queries.md)
- [接続パラメータ](connection-parameters.md)
- [予定クエリ](scheduled-queries.md)
- [スケジュールされたクエリに対して実行](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)

## 次の手順

これで、 [!DNL Query Service] APIを使用した呼び出しの実行方法を学んだので、独自の非インタラクティブクエリを作成できます。 クエリの作成方法の詳細については、『 [SQLリファレンスガイド](../sql/overview.md)』を参照してください。