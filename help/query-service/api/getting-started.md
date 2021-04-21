---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；クエリ
solution: Experience Platform
title: クエリサービスAPIガイド
topic-legacy: query templates
description: クエリサービスAPIを使用すると、開発者は標準SQLを使用してAdobe Experience Platformデータのクエリを行うことができます。 このガイドに従って、APIを使用した主な操作の実行方法を学習します。
exl-id: 2f4a156b-5623-419a-a9b2-72310f755708
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 42%

---

# [!DNL Query Service] API ガイド

この開発者ガイドは、Adobe Experience Platform[!DNL Query Service] APIの様々な操作を実行する手順を提供します。

## はじめに

このガイドでは、[!DNL Query Service]の使用に関わる様々なAdobe Experience Platformサービスについて、十分に理解しておく必要があります。

- [[!DNL Query Service]](../home.md):データセットをクエリし、結果のクエリをの新しいデータセットとして取り込む機能を提供 [!DNL Experience Platform]します。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
- [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、APIを使用して[!DNL Query Service]を正しく使用するために知っておく必要のある追加情報について説明します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。このドキュメントでサンプル API 呼び出しに使用される表記規則について詳しくは、 トラブルシューティングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

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
