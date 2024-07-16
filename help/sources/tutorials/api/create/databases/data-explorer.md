---
keywords: Experience Platform；ホーム；人気のトピック；Azure Azure Data Explorer;Azure Data Explorer;Azure Data Explorer
solution: Experience Platform
title: Flow Service API を使用した Azure Data Explorerベース接続の作成
type: Tutorial
description: Flow Service API を使用して Azure Data ExplorerをAdobe Experience Platformに接続する方法について説明します。
exl-id: 1b17bbb0-1f7b-4d89-a158-ad269e6edf30
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 62%

---

# [!DNL Flow Service] API を使用した [!DNL Azure Azure Data Explorer] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Azure Data Explorer] のベース接続を作成する手順を説明します。


## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Azure Data Explorer] ています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Azure Data Explorer] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL Azure Data Explorer] サーバーのエンドポイント。 |
| `database` | [!DNL Azure Data Explorer] データベースの名前。 |
| `tenant` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のテナント ID。 |
| `servicePrincipalId` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパル ID。 |
| `servicePrincipalKey` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパルキー。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Azure Data Explorer] の接続仕様 ID は `0479cc14-7651-4354-b233-7480606c2ac3` です。 |

はじめに詳しくは、この [[!DNL Azure Data Explorer]  ドキュメント ](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad) を参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Azure Data Explorer] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Azure Data Explorer] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Azure Azure Data Explorer connection",
        "description": "A connection for Azure Azure Data Explorer",
        "auth": {
            "specName": "Service Principal Based Authentication",
            "params": {
                    "endpoint": "{ENDPOINT}",
                    "database": "{DATABASE}",
                    "tenant": "{TENANT}",
                    "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
                    "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}"
                }
        },
        "connectionSpec": {
            "id": "0479cc14-7651-4354-b233-7480606c2ac3",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.endpoint` | [!DNL Azure Data Explorer] サーバーのエンドポイント。 |
| `auth.params.database` | [!DNL Azure Data Explorer] データベースの名前。 |
| `auth.params.tenant` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のテナント ID。 |
| `auth.params.servicePrincipalId` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパル ID。 |
| `auth.params.servicePrincipalKey` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパルキー。 |
| `connectionSpec.id` | [!DNL Azure Data Explorer] 接続仕様 ID: `0479cc14-7651-4354-b233-7480606c2ac3`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Azure Data Explorer] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
