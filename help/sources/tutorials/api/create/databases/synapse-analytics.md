---
title: Flow Service API を使用したAzure Synapse Analytics とExperience Platformの接続
description: API を使用してAzure Synapse Analytics アカウントをExperience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 8944ac3f-366d-49c8-882f-11cd0ea766e4
source-git-commit: b8497ede7f90717ed23bcd10b7abe51de18e08a5
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 15%

---

# [!DNL Flow Service] API を使用した [!DNL Azure Synapse Analytics] のExperience Platformへの接続

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Synapse Analytics] ソースを利用できます。

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Synapse Analytics] アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Azure Synapse Analytics] ています。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Azure Synapse Analytics]  概要 ](../../../../connectors/databases/synapse-analytics.md#prerequisites) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## [!DNL Azure Synapse Analytics] をExperience Platformに接続

ベース接続を作成し [!DNL Azure Synapse Analytics] アカウントをExperience Platformに接続する方法については、以下をお読みください。

### ベース接続の作成

**ベース接続** には、ソースシステムをAdobe Experience Platformにリンクする主要な情報が格納されます。 これには以下が含まれます。

* ソースの認証資格情報
* 接続の現在のステータス
* 一意の **ベース接続 ID**

**ベース接続 ID** を使用すると、ソースからファイルを参照して探索し、取り込む項目とそのデータタイプおよび形式を特定するのに役立ちます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを送信し、リクエストパラメーターに [!DNL Azure Synapse Analytics] 認証資格情報を含めます。

**API 形式**

```https
POST /connections
```

>[!BEGINTABS]

>[!TAB  接続文字列ベースの認証 ]

**リクエスト**

次のリクエストは、接続文字列ベースの認証を使用して、[!DNL Azure Synapse Analytics] のベース接続を作成します。

+++リクエスト例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Connection for Azure Synapse Analytics",
      "description": "Connection for Azure Synapse Analytics",
      "auth": {
          "specName": "Connection String Based Authentication",
          "params": {
              "connectionString": "Server=tcp:{SERVER_NAME}.database.windows.net,1433;Database={DATABASE};User ID={USERNAME}@{SERVER_NAME};Password={PASSWORD};Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
          }
      },
      "connectionSpec": {
          "id": "a49bcc7d-8038-43af-b1e4-5a7a089a7d79",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL Azure Synapse Analytics] への接続に使用する接続文字列。 [!DNL Azure Synapse Analytics] の接続文字列パターンは `Server=tcp:{SERVER_NAME}.database.windows.net,1433;Database={DATABASE};User ID={USERNAME}@{SERVER_NAME};Password={PASSWORD};Trusted_Connection=False;Encrypt=True;Connection Timeout=30` です。 |
| `connectionSpec.id` | [!DNL Azure Synapse Analytics] 接続仕様 ID は `a49bcc7d-8038-43af-b1e4-5a7a089a7d79` です。 |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

+++

>[!TAB  サービスプリンシパルキーベースの認証 ]

次のリクエストは、サービスプリンシパルキーベースの認証を使用して、[!DNL Azure Synapse Analytics] のベース接続を作成します。

**リクエスト**

+++リクエスト例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Connection for Azure Synapse Analytics",
    "description": "Connection for Azure Synapse Analytics",
    "auth": {
      "specName": "Service Principal Key Based Authentication",
      "params": {
        "server": "yourworkspace.sql.azuresynapse.net",
        "database": "SalesDW",
        "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
        "servicePrincipalId": "e7b8c1f2-1234-4c9a-9f3e-abcdef123456",
        "servicePrincipalKey": "~XyZ1234abcDEF5678..."
      }
    },
    "connectionSpec": {
      "id": "a49bcc7d-8038-43af-b1e4-5a7a089a7d79",
      "version": "1.0"
    }
  }'
```

| 資格情報 | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL Azure Synapse Analytics] SQL エンドポイントの完全修飾ドメイン名。 |
| `auth.params.database` | [!DNL Azure Synapse Analytics] ワークスペース内の特定のデータベースの名前。 |
| `auth.params.tenant` | [!DNL Azure] サブスクリプションに関連付けられた [!DNL Azure Active Directory] テナント ID。 |
| `auth.params.servicePrincipalId` | [!DNL Azure Active Directory] アプリケーションのクライアント ID。 |
| `auth.params.servicePrincipalKey` | サービスプリンシパルに関連付けられたクライアントの秘密鍵またはパスワード。 |
| `connectSpec.id` | [!DNL Azure Synapse Analytics] の接続仕様 ID |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

+++

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Azure Synapse Analytics] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)
