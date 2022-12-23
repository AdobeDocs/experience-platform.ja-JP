---
keywords: Experience Platform;ホーム;人気のトピック;query service;Query service;クエリ
solution: Experience Platform
title: Query Service API ガイド
topic-legacy: query templates
description: Query Service API を使用すると、開発者は標準の SQL を使用して Adobe Experience Platform データに対してクエリを実行できます。このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: 2f4a156b-5623-419a-a9b2-72310f755708
source-git-commit: 9f458a327c0b72a5984161f13f02d09b7a2e610e
workflow-type: ht
source-wordcount: '399'
ht-degree: 100%

---

# [!DNL Query Service] API ガイド

この開発者ガイドでは、Adobe Experience Platform [!DNL Query Service] API で様々な操作を実行する手順について説明します。

## はじめに

このガイドは、[!DNL Query Service] の使用に関連する様々な Adobe Experience Platform サービスを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Query Service]](../home.md)： データセットに対してクエリを実行し、結果のクエリを [!DNL Experience Platform] の新しいデータセットとして取得する機能を提供します。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
- [[!DNL Sandboxes]](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、 API を使用して [!DNL Query Service] を正常に使用するために必要な追加情報を示しています。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。このドキュメントでサンプル API 呼び出しに使用される表記規則について詳しくは、 トラブルシューティングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの使用について詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## サンプル API 呼び出し

使用するヘッダーを理解できたので、[!DNL Query Service] API への呼び出しを開始できます。次のドキュメントでは、[!DNL Query Service] API を使用して作成できる様々な API 呼び出しについて説明します。各呼び出し例では一般的な API 形式、必須ヘッダーを示すサンプルリクエストおよびサンプルレスポンスが示されています。

- [クエリ](queries.md)
- [接続パラメーター](connection-parameters.md)
- [スケジュール済みクエリ](scheduled-queries.md)
- [スケジュールされたクエリの実行](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)
- [高速クエリ](./accelerated-queries.md)
- [アラート購読](./alert-subscriptions.md)

## 次の手順

これで、[!DNL Query Service] API を使用した呼び出しの実行方法がわかったので、独自の非インタラクティブクエリを作成できます。クエリの作成方法について詳しくは、[SQL リファレンスガイド](../sql/overview.md)を参照してください。
