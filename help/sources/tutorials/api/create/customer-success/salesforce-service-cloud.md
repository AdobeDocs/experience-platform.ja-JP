---
title: Flow Service API を使用したSalesforce Service Cloud Source接続の作成
description: Flow Service API を使用してAdobe Experience PlatformをSalesforce Service Cloud に接続する方法について説明します。
exl-id: ed133bca-8e88-4c85-ae52-c3269b6bf3c9
source-git-commit: eab6303a3b420d4622185316922d242a4ce8a12d
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 24%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce Service Cloud] ソース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Salesforce Service Cloud] のベース接続を作成する方法については、このチュートリアルをお読みください。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md):Experience Platformには、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Salesforce Service Cloud] ています。

### 必要な資格情報の収集

>[!WARNING]
>
>[!DNL Salesforce Service Cloud] ソースの基本認証は、2026 年 1 月に廃止されます。 ソースの使用と [!DNL Salesforce Service Cloud] アカウントからExperience Platformへのデータの取り込みを続行するには、OAuth 2 クライアント資格情報認証に移行する必要があります。

[!DNL Salesforce Service Cloud] ソースは、基本認証と OAuth2 クライアント資格情報をサポートしています。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用して [!DNL Salesforce Service Cloud] アカウントを [!DNL Flow Service] に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce Service Cloud] ソースインスタンスの URL。 |
| `username` | [!DNL Salesforce Service Cloud] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Salesforce Service Cloud] ユーザーアカウントのパスワード。 |
| `securityToken` | [!DNL Salesforce Service Cloud] ユーザーアカウントのセキュリティ トークン。 |
| `apiVersion` | （オプション）使用している [!DNL Salesforce Service Cloud] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新バージョンが自動的に使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Service Cloud] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

基本について詳しくは、[ このSalesforceのドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm) を参照してください。

>[!TAB OAuth 2 クライアント資格情報 ]

OAuth 2 クライアント資格情報を使用して [!DNL Salesforce Service Cloud] アカウントを [!DNL Flow Service] に接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | [!DNL Salesforce Service Cloud] ソースインスタンスの URL。 |
| `clientId` | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce Service Cloud] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| `clientSecret` | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、[!DNL Salesforce Service Cloud] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションが動作することができます。 |
| `apiVersion` | 使用している [!DNL Salesforce Service Cloud] インスタンスの REST API バージョン。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョン `52` を使用している場合、値を `52.0` と入力する必要があります。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新バージョンが自動的に使用されます。 この値は、OAuth2 クライアント資格情報認証に必須です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Service Cloud] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

[!DNL Salesforce Service Cloud] に対する OAuth の使用について詳しくは、[[!DNL Salesforce Service Cloud] OAuth 認証フローのガイド ](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&type=5) を参照してください。

>[!ENDTABS]

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Salesforce Service Cloud] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!BEGINTABS]

>[!TAB  基本認証 ]

次のリクエストは、基本認証を使用して [!DNL Salesforce Service Cloud] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Salesforce Service Cloud account for ACME data (basic auth)",
      "description": "Salesforce Service Cloud account for ACME data (basic auth)",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
            "environmentUrl": "https://acme-enterprise-3126.my.salesforce.com",
            "username": "acme-salesforce-service-cloud",
            "password": "xxxx",
            "securityToken": "xxxx"
        }
      },
      "connectionSpec": {
          "id": "cb66ab34-8619-49cb-96d1-39b37ede86ea",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| ---| --- |
| `auth.params.environmentUrl` | [!DNL Salesforce Service Cloud] インスタンスの URL。 |
| `auth.params.username` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたパスワード。 |
| `auth.params.securityToken` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたセキュリティトークン。 |
| `connectionSpec.id` | [!DNL Salesforce Service Cloud] 接続仕様 ID：`cb66ab34-8619-49cb-96d1-39b37ede86ea` |

>[!TAB OAuth2 クライアント資格情報 ]

次のリクエストは、OAuth 2 クライアント資格情報を使用して、[!DNL Salesforce Service Cloud] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Salesforce Service Cloud account for ACME data (OAuth2)",
      "description": "Salesforce Service Cloud account for ACME data (OAuth2)",
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
          "id": "cb66ab34-8619-49cb-96d1-39b37ede86ea",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.environmentUrl` | [!DNL Salesforce Service Cloud] インスタンスの URL。 |
| `auth.params.clientId` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたクライアントの秘密鍵。 |
| `auth.params.apiVersion` | 使用している [!DNL Salesforce Service Cloud] インスタンスの REST API バージョン。 |
| `connectionSpec.id` | [!DNL Salesforce Service Cloud] 接続仕様 ID: `cb66ab34-8619-49cb-96d1-39b37ede86ea`。 |

>[!ENDTABS]

**応答**

正常な応答では、新しく作成したベース接続と、一意の ID が返されます。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce Service Cloud] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、カスタマーサクセスデータをExperience Platformに取り込むデータフローの作成](../../collect/customer-success.md)
