---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure AzureData Explorer;AzureData Explorer;AzureData Explorer
solution: Experience Platform
title: フローサービス API を使用した Azure AzureData Explorerベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Azure AzureData ExplorerをAdobe Experience Platformに接続する方法を説明します。
exl-id: 1b17bbb0-1f7b-4d89-a158-ad269e6edf30
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 10%

---

# [!DNL Flow Service] API を使用して [!DNL Azure Azure Data Explorer] ベース接続を作成する

>[!NOTE]
>
>[!DNL Azure Azure Data Explorer] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Data Explore] の基本接続を作成する手順を説明します。


## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Azure Data Explorer] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Azure Data Explorer] と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL Azure Data Explorer] サーバのエンドポイント。 |
| `database` | [!DNL Azure Data Explorer] データベースの名前。 |
| `tenant` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のテナント ID。 |
| `servicePrincipalId` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパル ID。 |
| `servicePrincipalKey` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパルキー。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Azure Data Explorer] の接続仕様 ID は `0479cc14-7651-4354-b233-7480606c2ac3` です。 |

使い始める方法については、この [[!DNL Azure Data Explorer]  ドキュメント ](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Azure Data Explorer] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `auth.params.endpoint` | [!DNL Azure Data Explorer] サーバのエンドポイント。 |
| `auth.params.database` | [!DNL Azure Data Explorer] データベースの名前。 |
| `auth.params.tenant` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のテナント ID。 |
| `auth.params.servicePrincipalId` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパル ID。 |
| `auth.params.servicePrincipalKey` | [!DNL Azure Data Explorer] データベースへの接続に使用する一意のサービスプリンシパルキー。 |
| `connectionSpec.id` | [!DNL Azure Data Explorer] 接続仕様 ID:`0479cc14-7651-4354-b233-7480606c2ac3`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Azure Data Explorer] 接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルでフローサービス API](../../explore/database-nosql.md) を使用してデータベースを調べる方法を学ぶ際に使用できます。[
