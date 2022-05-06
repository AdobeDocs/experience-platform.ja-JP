---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure AzureData Explorer;AzureData Explorer;AzureData Explorer
solution: Experience Platform
title: フローサービス API を使用した Azure AzureData Explorerベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Azure AzureData ExplorerをAdobe Experience Platformに接続する方法を説明します。
exl-id: 1b17bbb0-1f7b-4d89-a158-ad269e6edf30
source-git-commit: 93061c84639ca1fdd3f7abb1bbd050eb6eebbdd6
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 51%

---

# の作成 [!DNL Azure Azure Data Explorer] を使用したベース接続 [!DNL Flow Service] API

>[!NOTE]
>
>この [!DNL Azure Azure Data Explorer] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Azure Data Explorer] のベース接続を作成する手順を説明します。


## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Azure Data Explorer] の使用 [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Azure Data Explorer] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | のエンドポイント [!DNL Azure Data Explorer] サーバー。 |
| `database` | の名前 [!DNL Azure Data Explorer] データベース。 |
| `tenant` | への接続に使用される一意のテナント ID [!DNL Azure Data Explorer] データベース。 |
| `servicePrincipalId` | に接続するために使用される一意のサービスプリンシパル ID [!DNL Azure Data Explorer] データベース。 |
| `servicePrincipalKey` | への接続に使用する一意のサービスプリンシパルキー [!DNL Azure Data Explorer] データベース。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Azure Data Explorer] が `0479cc14-7651-4354-b233-7480606c2ac3`. |

導入の詳細については、こちらを参照してください。 [[!DNL Azure Data Explorer] 文書](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad).

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
| `auth.params.endpoint` | のエンドポイント [!DNL Azure Data Explorer] サーバー。 |
| `auth.params.database` | の名前 [!DNL Azure Data Explorer] データベース。 |
| `auth.params.tenant` | への接続に使用される一意のテナント ID [!DNL Azure Data Explorer] データベース。 |
| `auth.params.servicePrincipalId` | に接続するために使用される一意のサービスプリンシパル ID [!DNL Azure Data Explorer] データベース。 |
| `auth.params.servicePrincipalKey` | への接続に使用する一意のサービスプリンシパルキー [!DNL Azure Data Explorer] データベース。 |
| `connectionSpec.id` | この [!DNL Azure Data Explorer] 接続仕様 ID: `0479cc14-7651-4354-b233-7480606c2ac3`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Azure Data Explorer] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)
