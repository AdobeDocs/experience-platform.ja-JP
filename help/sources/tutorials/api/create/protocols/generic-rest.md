---
keywords: Experience Platform；ホーム；人気のトピック；汎用 REST；汎用 rest
solution: Experience Platform
title: Flow Service API を使用した汎用 REST API ベース接続の作成
type: Tutorial
description: Flow Service API を使用して汎用の REST API をAdobe Experience Platformに接続する方法を説明します。
exl-id: 6b414868-503e-49d5-8f4a-5b2fc003dab0
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 45%

---

# [!DNL Flow Service] API を使用した汎用 REST API ベース接続の作成

>[!NOTE]
>
>[!DNL Generic REST API] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[&#x200B; ソースの概要 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Generic REST API] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Generic REST API] に接続するには、選択した認証タイプの有効な資格情報を指定する必要があります。 [!DNL Generic REST API] は、OAuth 2 更新コードと基本認証の両方をサポートしています。 次の表に、サポートされる 2 つの認証タイプの資格情報を示します。

#### OAuth 2 更新コード

| 資格情報 | 説明 |
| --- | --- |
| `host` | リクエスト先のソースのホスト URL。 この値は必須であり、`requestParameterOverride` を使用してバイパスすることはできません。 |
| `authorizationTestUrl` | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用されます。指定しない場合、代わりにソース接続の作成時に資格情報が自動的にチェックされます。 |
| `clientId` | （オプション）ユーザーアカウントに関連付けられたクライアント ID。 |
| `clientSecret` | （オプション）ユーザーアカウントに関連付けられたクライアントの秘密鍵。 |
| `accessToken` | アプリケーションへのアクセスに使用するプライマリ認証情報。 アクセストークンは、ユーザーのデータの特定の側面にアクセスするための、アプリケーションの認証を表します。 この値は必須であり、`requestParameterOverride` を使用してバイパスすることはできません。 |
| `refreshToken` | （オプション）アクセストークンの有効期限が切れた場合に、新しいアクセストークンの生成に使用されるトークン。 |
| `expirationDate` | （オプション）アクセストークンの有効期限を定義する非表示の値。 |
| `accessTokenUrl` | （任意）アクセストークンの取得に使用する URL エンドポイント。 |
| `requestParameterOverride` | （オプション）上書きする資格情報パラメーターを指定できるプロパティ。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Generic REST API] の接続仕様 ID は `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62` です。 |

#### 基本認証

| 資格情報 | 説明 |
| --- | --- |
| `host` | リクエスト先のソースのホスト URL。 |
| `username` | ユーザーアカウントに対応するユーザー名。 |
| `password` | ユーザーアカウントに対応するパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Generic REST API] の接続仕様 ID は `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62` です。 |

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

[!DNL Generic REST API] は、基本認証と OAuth 2 更新コードの両方をサポートしています。いずれかの認証タイプで認証する方法については、次の例を参照してください。

### OAuth 2 更新コードコードを使って [!DNL Generic REST API] ベース接続を作成します

OAuth 2 更新コードを使用してベース接続 ID を作成するには、OAuth 2 資格情報を提供したうえで、`/connections` エンドポイントに対して POST リクエストを行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Generic REST API] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Generic REST API base connection with OAuth 2 refresh code",
      "description": "Generic REST API base connection with OAuth 2 refresh code",
      "connectionSpec": {
          "id": "4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62",
          "version": "1.0"
      },
      "auth": {
          "specName": "oAuth2RefreshCode",
          "params": {
              "host": "{HOST}",
              "accessToken": "{ACCESS_TOKEN}"
          }
      }
  }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `name` | ベース接続の名前。ベース接続の情報を検索する際に使用できるので、ベース接続の名前はわかりやすいものにしてください。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | 関連付けられた接続仕様 ID[!DNL Generic REST API] この修正済み ID は `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62` です。 |
| `auth.specName` | Experience Platformに対するソースの認証に使用する認証タイプ。 |
| `auth.params.host` | [!DNL Generic REST API] ソースへの接続に使用するルート URL。 |
| `auth.params.accessToken` | ソースの認証に使用された、対応するアクセストークン。これは、OAuth ベースの認証に必要です。 |

**応答**

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
  "id": "a5c6b647-e784-4b58-86b6-47e784ab580b",
  "etag": "\"7b01056a-0000-0200-0000-5e8a4f5b0000\""
}
```

### 基本認証を使用した [!DNL Generic REST API] ベース接続の作成

基本認証を使用した [!DNL Generic REST API] ベース接続を作成するには、基本認証資格情報を提供 [!DNL Flow Service] ながら、API の `/connections` エンドポイントに POST リクエストを行います。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Generic REST API] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "Generic REST API base connection with basic authentication",
      "description": "Generic REST API base connection with basic authentication",
      "connectionSpec": {
          "id": "4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62",
          "version": "1.0"
      },
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "host": "{HOST}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}"
          }
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。ベース接続の情報を検索する際に使用できるので、ベース接続の名前はわかりやすいものにしてください。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | 関連付けられた接続仕様 ID[!DNL Generic REST API] この修正済み ID は `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62` です。 |
| `auth.specName` | ソースをExperience Platformに接続するために使用する認証タイプ。 |
| `auth.params.host` | [!DNL Generic REST API] ソースへの接続に使用するルート URL。 |
| `auth.params.username` | [!DNL Generic REST API] ソースに対応するユーザー名。 これは、基本認証に必要です。 |
| `auth.params.password` | [!DNL Generic REST API] ソースに対応するパスワード。 これは、基本認証に必要です。 |

**応答**

リクエストが成功した場合は、一意の接続識別子（`id`）を含む、新しく作成されたベース接続が返されます。この ID は、次の手順でソースのファイル構造と内容を調べるために必要です。

```json
{
    "id": "9601747c-6874-4c02-bb00-5732a8c43086",
    "etag": "\"3702dabc-0000-0200-0000-615b5b5a0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Generic REST API] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [&#x200B; [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [&#x200B; [!DNL Flow Service] API を使用した、プロトコルデータをExperience Platformに取り込むデータフローの作成](../../collect/protocols.md)
