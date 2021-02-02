---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；クエリ
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: query templates
description: この開発者ガイドでは、Adobe Experience Platform のクエリサービス API で様々な操作を実行する手順を説明します。
translation-type: tm+mt
source-git-commit: ece2ae1eea8426813a95c18096c1b428acfd1a71
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 37%

---


# [!DNL Query Service] 開発ガイド

この開発者ガイドは、Adobe Experience Platform[!DNL Query Service] APIの様々な操作を実行する手順を提供します。

## はじめに

このガイドでは、[!DNL Query Service]の使用に関わる様々なAdobe Experience Platformサービスについて、十分に理解しておく必要があります。

- [[!DNL Query Service]](../home.md):データセットをクエリし、結果のクエリをの新しいデータセットとして取り込む機能を提供 [!DNL Experience Platform]します。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
- [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、APIを使用して[!DNL Query Service]を正しく使用するために知っておく必要のある追加情報について説明します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。このドキュメントでサンプル API 呼び出しに使用される表記規則について詳しくは、 トラブルシューティングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Experience Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform]でのサンドボックスの操作について詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

## サンプル API 呼び出し

これで、使用するヘッダーがわかったので、[!DNL Query Service] APIを呼び出す準備が整いました。 以下のドキュメントでは、[!DNL Query Service] APIを使用して実行できる様々なAPI呼び出しについて説明します。 各サンプル呼び出しには、一般的な API 形式、必要なヘッダーを示すサンプルリクエスト、サンプルの応答が含まれています。

- [クエリ](queries.md)
- [接続パラメータ](connection-parameters.md)
- [スケジュールクエリ](scheduled-queries.md)
- [スケジュールクエリの実行](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)

## 次の手順

これで、[!DNL Query Service] APIを使用した呼び出し方法を学んだので、独自の非インタラクティブクエリを作成できます。 クエリの作成方法について詳しくは、[SQL リファレンスガイド](../sql/overview.md)を参照してください。