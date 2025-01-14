---
title: Flow Service API を使用したSalesforceのExperience Platformへの接続
description: Flow Service API を使用してAdobe Experience PlatformをSalesforce アカウントに接続する方法について説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
source-git-commit: 01f655df8679383f57d60796be5274acd9b5df68
workflow-type: tm+mt
source-wordcount: '1077'
ht-degree: 24%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce] のExperience Platformへの接続

[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL Salesforce] ソースアカウントをAdobe Experience Platformに接続する方法については、このガイドを参照してください。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

## [!DNL Salesforce] を [!DNL Azure] のExperience Platformに接続する {#azure}

[!DNL Azure] で [!DNL Salesforce] ソースをExperience Platformに接続する方法については、以下の手順を参照してください。

### 必要な資格情報の収集

[!DNL Salesforce] ソースは、基本認証と OAuth2 クライアント資格情報をサポートしています。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用して [!DNL Salesforce] アカウントを [!DNL Flow Service] に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce] ソースインスタンスの URL。 `environmentUrl` の形式は `https://[domain].my.salesforce.com` です。 |
| `username` | [!DNL Salesforce] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Salesforce] ユーザーアカウントのパスワード。 |
| `securityToken` | [!DNL Salesforce] ユーザーアカウントのセキュリティ トークン。 |
| `apiVersion` | （オプション）使用している [!DNL Salesforce] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

基本について詳しくは、[ このSalesforceのドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm) を参照してください。

>[!TAB OAuth 2 クライアント資格情報 ]

OAuth 2 クライアント資格情報を使用して [!DNL Salesforce] アカウントを [!DNL Flow Service] に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce] ソースインスタンスの URL。 `environmentUrl` の形式は `https://[domain].my.salesforce.com` です |
| `clientId` | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| `clientSecret` | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| `apiVersion` | 使用している [!DNL Salesforce] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 この値は、OAuth2 クライアント資格情報認証に必須です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

[!DNL Salesforce] に対する OAuth の使用について詳しくは、[[!DNL Salesforce] OAuth 認証フローのガイド ](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&amp;type=5) を参照してください。

>[!ENDTABS]

### [!DNL Azure] のExperience Platformに [!DNL Salesforce] のベース接続を作成する

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続を作成し、[!DNL Salesforce] アカウントを [!DNL Azure] のExperience Platformに接続するには、`/connections` エンドポイントに対してPOSTリクエストを実行し、リクエスト本文に [!DNL Salesforce] 認証資格情報を指定します。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB  基本認証 ]

+++リクエスト

次のリクエストは、基本認証を使用して [!DNL Salesforce] のベース接続を作成します。

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
      "description": "Salesforce account using basic authentication",
      "auth": {
          "specName": "Basic Authentication",
          "params":
            "environmentUrl": "https://acme-enterprise-3126.my.salesforce.com",
            "username": "acme-salesforce",
            "password": "xxxx",
            "securityToken": "xxxx"
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
| `auth.params.environmentUrl` | [!DNL Salesforce] インスタンスの URL。 |
| `auth.params.username` | [!DNL Salesforce] アカウントに関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL Salesforce] アカウントに関連付けられたパスワード。 |
| `auth.params.securityToken` | [!DNL Salesforce] アカウントに関連付けられたセキュリティトークン。 |
| `connectionSpec.id` | [!DNL Salesforce] 接続仕様 ID: `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`。 |

+++

+++応答

正常な応答では、新しく作成したベース接続と、一意の ID が返されます。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

+++

>[!TAB OAuth 2 クライアント資格情報 ]

+++リクエスト

次のリクエストは、OAuth 2 クライアント資格情報を使用して、[!DNL Salesforce] のベース接続を作成します。

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
            "apiVersion": "60.0"
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
| `auth.params.environmentUrl` | [!DNL Salesforce] インスタンスの URL。 |
| `auth.params.clientId` | [!DNL Salesforce] アカウントに関連付けられたクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Salesforce] アカウントに関連付けられたクライアントの秘密鍵。 |
| `auth.params.apiVersion` | 使用している [!DNL Salesforce] インスタンスの REST API バージョン。 |
| `connectionSpec.id` | [!DNL Salesforce] 接続仕様 ID: `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`。 |

+++


+++応答

正常な応答では、新しく作成したベース接続と、一意の ID が返されます。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

+++

>[!ENDTABS]

## [!DNL Salesforce] をAmazon Web ServicesのExperience Platform（AWS）に接続する {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWSで実行されるExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platformインフラストラクチャについて詳しくは、[Experience Platformマルチクラウドの概要 ](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL Salesforce] ソースをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### 前提条件

AWSでExperience Platformに接続できるように [!DNL Salesforce] アカウントを設定する方法については、[[!DNL Salesforce]  概要 ](../../../../connectors/crm/salesforce.md#aws) を参照してください。

### AWSで [!DNL Salesforce] on Experience Platformのベース接続を作成する

ベース接続を作成し、[!DNL Salesforce] アカウントをAWS上のExperience Platformに接続するには、`/connections` エンドポイントに対してPOSTリクエストを実行し、資格情報の適切な値を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

+++選択してリクエストを表示

次のリクエストは、AWSのExperience Platformに [!DNL Salesforce] ソースのベース接続を作成します。

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

[!DNL Salesforce] `jwtToken` の取得方法について詳しくは、[AWSでExperience Platformに接続するソースの設定方法 ](../../../../connectors/crm/salesforce.md#aws) に関するガイドを参  [!DNL Salesforce]  してください。

+++

**応答**

+++選択して応答を表示

正常な応答では、新しく作成したベース接続と、一意の ID が返されます。

```json
{
    "id": "3e908d3f-c390-482b-9f44-43d3d4f2eb82",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

+++

### 接続ステータスの確認

接続ステータスを確認するには、`/connections` エンドポイントに対してGETリクエストを行い、作成手順で生成されたベース接続 ID を指定します。

**API 形式**

```http
GET /connections
```

**リクエスト**

+++選択してリクエストを表示

次のリクエストは、ベース接続 ID `3e908d3f-c390-482b-9f44-43d3d4f2eb82` の情報を取得します。

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

>[!TAB  初期化中 ]

+++選択すると応答の例が表示されます

次の応答では、`initializing` 状態の間にベース接続 ID `3e908d3f-c390-482b-9f44-43d3d4f2eb82` の情報が表示されます。

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

+++選択すると応答の例が表示されます

次の応答では、`enabled` 状態の間にベース接続 ID `3e908d3f-c390-482b-9f44-43d3d4f2eb82` の情報が表示されます。

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

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、CRM データを Platform に取り込むデータフローの作成](../../collect/crm.md)
