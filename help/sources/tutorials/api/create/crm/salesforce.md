---
title: Flow Service APIを使用してSalesforceをExperience Platformに接続する
description: Flow Service APIを使用してAdobe Experience PlatformをSalesforce アカウントに接続する方法を説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
source-git-commit: 11e9e1a25a45f4011f15b1e28753a98d4158012c
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 18%

---

# [!DNL Flow Service] APIを使用して[!DNL Salesforce]をExperience Platformに接続します

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/)を使用して[!DNL Salesforce] ソースアカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform APIの使用

Experience Platform APIの呼び出しを正常に行う方法について詳しくは、[Experience Platform APIの概要](../../../../../landing/api-guide.md)に関するガイドを参照してください。

## [!DNL Salesforce]を[!DNL Azure]にExperience Platformに接続します {#azure}

[!DNL Azure]で[!DNL Salesforce] ソースをExperience Platformに接続する方法について詳しくは、以下の手順を参照してください。

### 必要な資格情報の収集

[!DNL Salesforce] ソースは、OAuth2 クライアント資格情報による認証をサポートしています。

OAuth 2 クライアント資格情報を使用して[!DNL Salesforce] アカウントを[!DNL Flow Service]に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce] ソースインスタンスのURL。 `environmentUrl`の形式は`https://[domain].my.salesforce.com`です |
| `clientId` | クライアント IDは、OAuth2認証の一環として、クライアント秘密鍵と並行して使用されます。 クライアント IDとクライアント秘密鍵を組み合わせることで、アプリケーションを[!DNL Salesforce]に対して識別し、アカウントの代理でアプリケーションを操作できるようになります。 |
| `clientSecret` | クライアント秘密鍵は、OAuth2認証の一環として、クライアント IDと並行して使用されます。 クライアント IDとクライアント秘密鍵を組み合わせることで、アプリケーションを[!DNL Salesforce]に対して識別し、アカウントの代理でアプリケーションを操作できるようになります。 |
| `apiVersion` | 使用している[!DNL Salesforce] インスタンスのREST API バージョン。 API バージョンの値は、10進数でフォーマットする必要があります。 例えば、API バージョン `52`を使用している場合、値を`52.0`として入力する必要があります。 このフィールドを空白のままにすると、Experience Platformは使用可能な最新バージョンを自動的に使用します。 この値は、OAuth2 Client Credential認証に必須です。 |
| `includeDeletedObjects` | 削除されたレコードをソフトで含めるかどうかを判断するために使用されるブール値。 trueに設定すると、ソフト削除されたレコードを[!DNL Salesforce] クエリに含め、アカウントからExperience Platformに取り込むことができます。 設定を指定しない場合、この値はデフォルトで`false`になります。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 [!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

[!DNL Salesforce]でのOAuthの使用について詳しくは、[[!DNL Salesforce] OAuth認証フローに関するガイド ](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&type=5)を参照してください。

### [!DNL Azure]にExperience Platformの[!DNL Salesforce]のベース接続を作成します

ベース接続は、ソースの認証情報、接続の現在の状態、一意のベース接続IDなど、ソースとExperience Platform間の情報を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続を作成し、[!DNL Salesforce] アカウントを[!DNL Azure]のExperience Platformに接続するには、`/connections` エンドポイントにPOST リクエストを行い、リクエスト本文に[!DNL Salesforce]認証情報を入力します。

**API 形式**

```http
POST /connections
```

+++選択してリクエストを表示

次のリクエストは、OAuth 2 クライアント資格情報を使用して[!DNL Salesforce]のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ACME Salesforce account",
      "description": "Salesforce account using OAuth 2",
      "auth": {
          "specName": "OAuth2 Client Credential",
          "params":
            "environmentUrl": "https://acme-enterprise-3126.my.salesforce.com",
            "clientId": "xxxx",
            "clientSecret": "xxxx",
            "apiVersion": "60.0",
            "includeDeletedObjects": true
        }
      },
      "connectionSpec": {
          "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.environmentUrl` | [!DNL Salesforce] インスタンスのURL。 |
| `auth.params.clientId` | [!DNL Salesforce] アカウントに関連付けられているクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Salesforce] アカウントに関連付けられているクライアントの秘密鍵。 |
| `auth.params.apiVersion` | 使用している[!DNL Salesforce] インスタンスのREST API バージョン。 |
| `auth.params.includeDeletedObjects` | 削除されたレコードをソフトに含めるかどうかを判断するために使用されるブール値。 |
| `connectionSpec.id` | [!DNL Salesforce]接続仕様ID: `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`。 |

+++


+++選択して応答を表示

応答が成功すると、新しく作成したベース接続とその一意のIDが返されます。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

+++

## [!DNL Salesforce]をAmazon Web Services （AWS）上のExperience Platformに接続 {#aws}

>[!AVAILABILITY]
>
>この節は、Amazon Web Services（AWS）で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、一部のお客様にご利用いただけます。 サポートされているExperience Platform インフラストラクチャについて詳しくは、[Experience Platform マルチクラウドの概要](../../../../../landing/multi-cloud.md)を参照してください。

AWSで[!DNL Salesforce] ソースをExperience Platformに接続する方法について詳しくは、以下の手順を参照してください。

### 前提条件

AWSでExperience Platformに接続できるように[!DNL Salesforce] アカウントを設定する方法について詳しくは、[[!DNL Salesforce] 概要](../../../../connectors/crm/salesforce.md#aws)を参照してください。

### AWS上のExperience Platformで[!DNL Salesforce]のベース接続を作成する

ベース接続を作成し、AWS上のExperience Platformに[!DNL Salesforce] アカウントを接続するには、`/connections` エンドポイントにPOST リクエストを行い、資格情報に適切な値を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

+++選択してリクエストを表示

次のリクエストは、AWS上のExperience Platformで[!DNL Salesforce] ソースのベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ACME Salesforce account on AWS",
      "description": "ACME Salesforce account on AWS",
      "auth": {
          "specName": "OAuth2 JWT Token Credential",
          "params":
            "jwtToken": "{JWT_TOKEN},
            "clientId": "xxxx",
            "clientSecret": "xxxx",
            "instanceUrl": "https://acme-enterprise-3126.my.salesforce.com"
        }
      },
      "connectionSpec": {
          "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
          "version": "1.0"
      }
  }'
```

[!DNL Salesforce] `jwtToken`を取得する方法について詳しくは、[AWSでExperience Platformに接続するための [!DNL Salesforce]  ソースの設定方法](../../../../connectors/crm/salesforce.md#aws)に関するガイドを参照してください。

+++

**応答**

+++選択して応答を表示

応答が成功すると、新しく作成したベース接続とその一意のIDが返されます。

```json
{
    "id": "3e908d3f-c390-482b-9f44-43d3d4f2eb82",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

+++

### 接続ステータスの確認

接続ステータスを確認するには、`/connections` エンドポイントに対してGET リクエストを行い、作成手順で生成されたベース接続IDを指定します。

**API 形式**

```http
GET /connections
```

**リクエスト**

+++選択してリクエストを表示

次のリクエストは、ベース接続ID `3e908d3f-c390-482b-9f44-43d3d4f2eb82`の情報を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/3e908d3f-c390-482b-9f44-43d3d4f2eb82' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

>[!BEGINTABS]

>[!TAB 初期化中]

+++選択して応答の例を表示

次の応答は、`initializing`状態の間にベース接続ID `3e908d3f-c390-482b-9f44-43d3d4f2eb82`に関する情報を表示します。

```json
{
  "items": [
    {
      "id": "3e908d3f-c390-482b-9f44-43d3d4f2eb82",
      "createdAt": 1736506325115,
      "updatedAt": 1736506325717,
      "createdBy": "acme@techacct.adobe.com",
      "updatedBy": "acme@techacct.adobe.com",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{UPDATED_CLIENT}",
      "sandboxId": "{SANDBOX_ID}",
      "sandboxName": "{SANDBOX_NAME}",
      "imsOrgId": "{ORG_ID}",
      "name": "JWT Token Auth Authentication E2E-1736506322",
      "description": "Base Connection for salesforce E2E",
      "connectionSpec": {
        "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
        "version": "1.0"
      },
      "state": "initializing",
      "auth": {
        "specName": "OAuth2 JWT Token Credential",
        "params": {
          "jwtToken": "{JWT_TOKEN}",
          "clientId": "{CLIENT_ID}",
          "clientSecret": "{CLIENT_SECRET}",
          "instanceUrl": "https://acme-enterprise-3126.my.salesforce.com"
        }
      }
    }
  }
]
```

+++

>[!TAB 有効]

+++選択して応答の例を表示

次の応答は、`enabled`状態の間にベース接続ID `3e908d3f-c390-482b-9f44-43d3d4f2eb82`に関する情報を表示します。

```json
{
  "items": [
      {
        "id": "3e908d3f-c390-482b-9f44-43d3d4f2eb82",
        "createdAt": 1736506325115,
        "updatedAt": 1736506413299,
        "createdBy": "acme@techacct.adobe.com",
        "updatedBy": "acme@AdobeID",
        "createdClient": "{CREATED_CLIENT}",
        "updatedClient": "acme",
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "imsOrgId": "{ORG_ID}",
        "name": "JWT Token Auth Authentication E2E-1736506322",
        "description": "Base Connection for salesforce E2E",
        "connectionSpec": {
          "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
          "version": "1.0"
        },
        "state": "enabled",
        "auth": {
          "specName": "OAuth2 JWT Token Credential",
          "params": {
            "jwtToken": "{JWT_TOKEN}",
            "clientId": "{CLIENT_ID}",
            "clientSecret": "{CLIENT_SECRET}",
            "instanceUrl": "https://adb8-dev-ed.develop.my.salesforce.com",
            "orgId": "00DdL000001iPRxUAM"
          }
        },
        "version": "\"6d27f305-40be-41c3-97d4-a701827c34df\"",
        "etag": "\"6d27f305-40be-41c3-97d4-a701827c34df\""
    }
  ]
}
```

+++

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce] ベース接続を作成しました。 このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] APIを使用してCRM データをExperience Platformに取り込むデータフローを作成します](../../collect/crm.md)
