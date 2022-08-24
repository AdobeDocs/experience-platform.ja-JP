---
keywords: Experience Platform；ホーム；人気の高いトピック；bigquery;Google;google;Google BigQuery
solution: Experience Platform
title: フローサービス API を使用したGoogle BigQuery ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをGoogle BigQuery に接続する方法を説明します。
exl-id: 51f90366-7a0e-49f1-bd57-b540fa1d15af
source-git-commit: 015a4fa06fc2157bb8374228380bb31826add37e
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 53%

---

# [!DNL Flow Service] API を使用した [!DNL Google BigQuery] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Google BigQuery] のベース接続を作成する手順を説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Google BigQuery] の使用 [!DNL Flow Service] API

### 必要な認証情報の収集

次のために [!DNL Flow Service] 接続する [!DNL Google BigQuery] Platform では、次の OAuth 2.0 認証値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `project` | デフォルトのプロジェクト ID [!DNL Google BigQuery] クエリするプロジェクト。 |
| `clientID` | 更新トークンの生成に使用する ID 値。 |
| `clientSecret` | 更新トークンの生成に使用するシークレット値。 |
| `refreshToken` | から取得した更新トークン [!DNL Google] ～へのアクセスを許可するために使用される [!DNL Google BigQuery]. |
| `largeResultsDataSetId` | 事前作成済み  [!DNL Google BigQuery] 大きな結果セットのサポートを有効にするために必要なデータセット ID。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。[!DNL Google BigQuery] の接続仕様 ID は `3c9b37f8-13a6-43d8-bad3-b863b941fedd` です。 |

これらの値について詳しくは、 [[!DNL Google BigQuery] 文書](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Google BigQuery] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Google BigQuery] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google BigQuery connection",
        "description": "Google BigQuery connection",
        "auth": {
            "specName": "Basic Authentication",
            "type": "OAuth2.0",
            "params": {
                    "project": "{PROJECT}",
                    "clientId": "{CLIENT_ID},
                    "clientSecret": "{CLIENT_SECRET}",
                    "refreshToken": "{REFRESH_TOKEN}"
                }
        },
        "connectionSpec": {
            "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.project` | デフォルトのプロジェクト ID [!DNL Google BigQuery] クエリするプロジェクト。 対して |
| `auth.params.clientId` | 更新トークンの生成に使用する ID 値。 |
| `auth.params.clientSecret` | 更新トークンの生成に使用するクライアント値。 |
| `auth.params.refreshToken` | から取得した更新トークン [!DNL Google] ～へのアクセスを許可するために使用される [!DNL Google BigQuery]. |
| `connectionSpec.id` | この [!DNL Google BigQuery] 接続仕様 ID: `3c9b37f8-13a6-43d8-bad3-b863b941fedd`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6990abad-977d-41b9-a85d-17ea8cf1c0e4",
    "etag": "\"ca00acbf-0000-0200-0000-60149e1e0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Google BigQuery] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)
