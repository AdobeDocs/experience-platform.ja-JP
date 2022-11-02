---
keywords: Experience Platform;ホーム;人気のトピック;query service;Query service;クエリ
solution: Experience Platform
title: クエリサービス API ガイド
topic-legacy: query templates
description: クエリサービス API を使用すると、開発者は標準の SQL を使用してAdobe Experience Platformデータに対してクエリを実行できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: 2f4a156b-5623-419a-a9b2-72310f755708
source-git-commit: 9f458a327c0b72a5984161f13f02d09b7a2e610e
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 55%

---

# [!DNL Query Service] API ガイド

この開発者ガイドでは、Adobe Experience Platformで様々な操作を実行する手順を説明します [!DNL Query Service] API

## はじめに

このガイドでは、の使用に関連する様々なAdobe Experience Platformサービスに関する十分な知識が必要です [!DNL Query Service].

- [[!DNL Query Service]](../home.md):でデータセットに対してクエリを実行し、結果のクエリを新しいデータセットとして取り込む機能を提供します。 [!DNL Experience Platform].
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。
- [[!DNL Sandboxes]](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、を正しく使用するために知っておく必要がある追加情報を示します [!DNL Query Service] API を使用している場合にのみ表示されます。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。このドキュメントでサンプル API 呼び出しに使用される表記規則について詳しくは、 トラブルシューティングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。へのすべてのリクエスト [!DNL Platform] API には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>でのサンドボックスの操作について詳しくは、 [!DNL Experience Platform]を参照し、 [サンドボックスの概要ドキュメント](../../sandboxes/home.md).

## サンプル API 呼び出し

これで、使用するヘッダーを理解できたので、 [!DNL Query Service] API 以下のドキュメントでは、 [!DNL Query Service] API 各呼び出し例では一般的な API 形式、必須ヘッダーを示すサンプルリクエストおよびサンプルレスポンスが示されています。

- [クエリ](queries.md)
- [接続パラメーター](connection-parameters.md)
- [スケジュール済みクエリ](scheduled-queries.md)
- [スケジュールされたクエリの実行](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)
- [高速クエリ](./accelerated-queries.md)
- [アラート購読](./alert-subscriptions.md)

## 次の手順

これで、 [!DNL Query Service] API を使用すると、独自の非インタラクティブクエリを作成できます。 クエリの作成方法について詳しくは、[SQL リファレンスガイド](../sql/overview.md)を参照してください。
