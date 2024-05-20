---
title: Flow Service API を使用した Salesforce ベース接続の作成
description: Flow Service API を使用してAdobe Experience Platformを Salesforce アカウントに接続する方法について説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
source-git-commit: 8d62cf4ca0071e84baa9399e0a25f7ebfb096c1a
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 40%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Salesforce] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、正常に接続するために必要な追加情報を示します [!DNL Platform] から [!DNL Salesforce] を使用したアカウント [!DNL Flow Service] API です。

### 必要な資格情報の収集

この [!DNL Salesforce] ソースは、基本認証および OAuth2 クライアント資格情報をサポートします。

>[!BEGINTABS]

>[!TAB 基本認証]

を接続するには [!DNL Salesforce] 移動先の口座 [!DNL Flow Service] 基本認証を使用して、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | の URL [!DNL Salesforce] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Salesforce] ユーザーアカウント。 |
| `password` | パスワード： [!DNL Salesforce] ユーザーアカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Salesforce] ユーザーアカウント。 |
| `apiVersion` | （オプション）の REST API バージョン [!DNL Salesforce] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

基本について詳しくは、を参照してください。 [この Salesforce ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm).

>[!TAB OAuth 2 クライアント資格情報]

を接続するには [!DNL Salesforce] 移動先の口座 [!DNL Flow Service] oauth 2 クライアント資格情報を使用して、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | の URL [!DNL Salesforce] ソースインスタンス。 |
| `clientId` | クライアント ID は、OAuth2 認証の一部として、クライアント秘密鍵と並行して使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce]. |
| `clientSecret` | クライアントの秘密鍵は、クライアント ID と並行して、OAuth2 認証の一部として使用されます。 クライアント ID とクライアント秘密鍵を一緒に使用すると、アプリケーションを識別して、アカウントに代わってアプリケーションを動作させることができます [!DNL Salesforce]. |
| `apiVersion` | の REST API バージョン [!DNL Salesforce] 使用しているインスタンス。 API バージョンの値は、10 進数でフォーマットする必要があります。 例えば、API バージョンを使用している場合 `52`の場合、値をに入力する必要があります `52.0`. このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 この値は、OAuth2 クライアント資格情報認証に必須です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

用の OAuth の使用の詳細 [!DNL Salesforce]、を読み取ります [[!DNL Salesforce] oauth 認証フローのガイド](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&amp;type=5).

>[!ENDTABS]

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、次に対してPOSTリクエストを実行します。 `/connections` エンドポイントと提供 [!DNL Salesforce] リクエスト本文の認証資格情報。

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!BEGINTABS]

>[!TAB 基本認証]

次のリクエストは、のベース接続を作成します [!DNL Salesforce] 基本認証を使用：

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
| `auth.params.environmentUrl` | の URL [!DNL Salesforce] インスタンス。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL Salesforce] アカウント。 |
| `auth.params.password` | に関連付けられたパスワード [!DNL Salesforce] アカウント。 |
| `auth.params.securityToken` | に関連付けられたセキュリティトークン [!DNL Salesforce] アカウント。 |
| `connectionSpec.id` | この [!DNL Salesforce] 接続仕様 ID: `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`. |

>[!TAB OAuth 2 クライアント資格情報]

次のリクエストは、のベース接続を作成します [!DNL Salesforce] oauth 2 クライアント資格情報を使用：

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
| `auth.params.environmentUrl` | の URL [!DNL Salesforce] インスタンス。 |
| `auth.params.clientId` | に関連付けられたクライアント ID [!DNL Salesforce] アカウント。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce] アカウント。 |
| `auth.params.apiVersion` | の REST API バージョン [!DNL Salesforce] 使用しているインスタンス。 |
| `connectionSpec.id` | この [!DNL Salesforce] 接続仕様 ID: `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`. |

>[!ENDTABS]

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成された接続が応答として返されます。この ID は、次の手順で CRM システムを探索するために必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [を使用した、CRM データを Platform に取り込むデータフローの作成 [!DNL Flow Service] API](../../collect/crm.md)
