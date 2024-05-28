---
title: Flow Service API を使用した Salesforce サービスクラウドソース接続の作成
description: Flow Service API を使用してAdobe Experience Platformを Salesforce Service Cloud に接続する方法について説明します。
exl-id: ed133bca-8e88-4c85-ae52-c3269b6bf3c9
source-git-commit: 7930a869627130a5db34780e64b809cda0c1e5f4
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 33%

---

# を作成 [!DNL Salesforce Service Cloud] を使用したソース接続 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

のベース接続を作成する方法については、このチュートリアルをお読みください [!DNL Salesforce Service Cloud] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、受信データの構造化、ラベル付け、拡張を行うことができます [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformには、1 つをパーティション化する仮想サンドボックスが用意されています [!DNL Platform] 個別の仮想環境にインスタンス化して、デジタルエクスペリエンスアプリケーションの開発と発展を支援します。

次の節では、に正常に接続するために必要な追加情報を示します [!DNL Salesforce Service Cloud] の使用 [!DNL Flow Service] API です。

### 必要な資格情報の収集

この [!DNL Salesforce Service Cloud] ソースは、基本認証および OAuth2 クライアント資格情報をサポートします。

>[!BEGINTABS]

>[!TAB 基本認証]

を接続するには [!DNL Salesforce Service Cloud] 移動先の口座 [!DNL Flow Service] 基本認証を使用して、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | の URL [!DNL Salesforce Service Cloud] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| `password` | パスワード： [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| `apiVersion` | （オプション）の REST API バージョン [!DNL Salesforce Service Cloud] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Service Cloud] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

基本について詳しくは、を参照してください。 [この Salesforce ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm).

>[!TAB OAuth 2 クライアント資格情報]

を接続するには [!DNL Salesforce Service Cloud] 移動先の口座 [!DNL Flow Service] oauth 2 クライアント資格情報を使用して、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | の URL [!DNL Salesforce Service Cloud] ソースインスタンス。 |
| `clientId` | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce Service Cloud]. |
| `clientSecret` | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce Service Cloud]. |
| `apiVersion` | の REST API バージョン [!DNL Salesforce Service Cloud] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 この値は、OAuth2 クライアント資格情報認証に必須です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Service Cloud] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

用の OAuth の使用の詳細 [!DNL Salesforce Service Cloud]、を読み取ります [[!DNL Salesforce Service Cloud] oauth 認証フローのガイド](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&amp;type=5).

>[!ENDTABS]

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Salesforce Service Cloud] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!BEGINTABS]

>[!TAB 基本認証]

次のリクエストは、のベース接続を作成します [!DNL Salesforce Service Cloud] 基本認証を使用：

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
| `auth.params.environmentUrl` | の URL [!DNL Salesforce Service Cloud] インスタンス。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL Salesforce Service Cloud] アカウント。 |
| `auth.params.password` | に関連付けられたパスワード [!DNL Salesforce Service Cloud] アカウント。 |
| `auth.params.securityToken` | に関連付けられたセキュリティトークン [!DNL Salesforce Service Cloud] アカウント。 |
| `connectionSpec.id` | [!DNL Salesforce Service Cloud] 接続仕様 ID：`cb66ab34-8619-49cb-96d1-39b37ede86ea` |

>[!TAB OAuth2 クライアント資格情報]

次のリクエストは、のベース接続を作成します [!DNL Salesforce Service Cloud] oauth 2 クライアント資格情報を使用：

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
| `auth.params.environmentUrl` | の URL [!DNL Salesforce Service Cloud] インスタンス。 |
| `auth.params.clientId` | に関連付けられたクライアント ID [!DNL Salesforce Service Cloud] アカウント。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Service Cloud] アカウント。 |
| `auth.params.apiVersion` | の REST API バージョン [!DNL Salesforce Service Cloud] 使用しているインスタンス。 |
| `connectionSpec.id` | この [!DNL Salesforce Service Cloud] 接続仕様 ID: `cb66ab34-8619-49cb-96d1-39b37ede86ea`. |

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
* [ [!DNL Flow Service]  API を使用した、カスタマーサクセスデータを Platform に取り込むデータフローの作成](../../collect/customer-success.md)
