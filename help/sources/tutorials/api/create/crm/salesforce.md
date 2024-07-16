---
title: Flow Service API を使用した Salesforce ベース接続の作成
description: Flow Service API を使用してAdobe Experience Platformを Salesforce アカウントに接続する方法について説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
source-git-commit: 7d450ba3357389a2934f187e4838e534d698dd4a
workflow-type: tm+mt
source-wordcount: '774'
ht-degree: 37%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Salesforce] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Salesforce] アカウントに正しく接続するために必要 [!DNL Platform] 追加情報を示しています。

### 必要な資格情報の収集

[!DNL Salesforce] ソースは、基本認証と OAuth2 クライアント資格情報をサポートしています。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用して [!DNL Salesforce] アカウントを [!DNL Flow Service] に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce] ソースインスタンスの URL。 |
| `username` | [!DNL Salesforce] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Salesforce] ユーザーアカウントのパスワード。 |
| `securityToken` | [!DNL Salesforce] ユーザーアカウントのセキュリティ トークン。 |
| `apiVersion` | （オプション）使用している [!DNL Salesforce] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

基本について詳しくは、[ この Salesforce ドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm) を参照してください。

>[!TAB OAuth 2 クライアント資格情報 ]

OAuth 2 クライアント資格情報を使用して [!DNL Salesforce] アカウントを [!DNL Flow Service] に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce] ソースインスタンスの URL。 |
| `clientId` | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| `clientSecret` | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| `apiVersion` | 使用している [!DNL Salesforce] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 この値は、OAuth2 クライアント資格情報認証に必須です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

[!DNL Salesforce] に対する OAuth の使用について詳しくは、[[!DNL Salesforce] OAuth 認証フローのガイド ](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&amp;type=5) を参照してください。

>[!ENDTABS]

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対してPOSTリクエストを実行し、リクエスト本文に [!DNL Salesforce] 認証資格情報を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!BEGINTABS]

>[!TAB  基本認証 ]

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

>[!TAB OAuth 2 クライアント資格情報 ]

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

>[!ENDTABS]

**応答**

正常な応答では、新しく作成したベース接続と、一意の ID が返されます。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、CRM データを Platform に取り込むデータフローの作成](../../collect/crm.md)
