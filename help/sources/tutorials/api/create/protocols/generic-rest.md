---
keywords: Experience Platform；ホーム；人気のあるトピック；汎用 REST；汎用 REST
solution: Experience Platform
title: フローサービス API を使用した汎用 REST API ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して汎用 REST API をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 1a9c4d5ba3ba9201378e78c0e92dea5101668a24
workflow-type: tm+mt
source-wordcount: '939'
ht-degree: 6%

---

# 汎用 REST API ベース接続の作成 ( [!DNL Flow Service] API

>[!NOTE]
>
>10. [!DNL Generic REST API] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベルのコネクタの使用に関する詳細

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、の基本接続を作成する手順を説明します。 [!DNL Generic REST API] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

Platform API を正常に呼び出す方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

### 必要な資格情報の収集

次の順序で [!DNL Flow Service] ～とつながる [!DNL Generic REST API]の場合は、選択した認証タイプに対して有効な資格情報を指定する必要があります。 [!DNL Generic REST API] は、OAuth 2 更新コードと基本認証の両方をサポートしています。 次の表に、サポートされる 2 つの認証タイプの資格情報を示します。

#### OAuth 2 更新コード

| 資格情報 | 説明 |
| --- | --- |
| `host` | リクエスト先のソースのホスト URL。 この値は必須で、 `requestParameterOverride`. |
| `authorizationTestUrl` | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用します。 指定しない場合、代わりに、ソース接続の作成手順で資格情報が自動的にチェックされます。 |
| `clientId` | （オプション）ユーザーアカウントに関連付けられているクライアント ID。 |
| `clientSecret` | （オプション）ユーザーアカウントに関連付けられたクライアント秘密鍵。 |
| `accessToken` | アプリケーションへのアクセスに使用するプライマリ認証資格情報。 アクセストークンは、ユーザーのデータの特定の側面にアクセスするための、アプリケーションの認証を表します。 この値は必須で、 `requestParameterOverride`. |
| `refreshToken` | （オプション）アクセストークンの有効期限が切れたときに、新しいアクセストークンの生成に使用されるトークン。 |
| `expirationDate` | （オプション）アクセストークンの有効期限を定義する hidden 値です。 |
| `accessTokenUrl` | （オプション）アクセストークンの取得に使用する URL エンドポイント。 |
| `requestParameterOverride` | （オプション）上書きする秘密鍵証明書のパラメーターを指定できるプロパティ。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Generic REST API] 次の値になります。 `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62`. |

#### 基本認証

| 資格情報 | 説明 |
| --- | --- |
| `host` | リクエスト先のソースのホスト URL。 |
| `username` | ユーザーアカウントに対応するユーザー名。 |
| `password` | ユーザーアカウントに対応するパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Generic REST API] 次の値になります。 `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62`. |

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

[!DNL Generic REST API] は、基本認証と OAuth 2 更新コードの両方をサポートしています。 次の例で、いずれかの認証タイプを使用して認証する方法に関するガイダンスを参照してください。

### の作成 [!DNL Generic REST API] OAuth 2 更新コードを使用したベース接続

OAuth 2 更新コードを使用してベース接続 ID を作成するには、 `/connections` エンドポイントで OAuth 2 資格情報を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Generic REST API]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `name` | ベース接続の名前。 ベース接続の名前がわかりやすいことを確認します。これを使用して、ベース接続の情報を検索できます。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | 関連付けられている接続仕様 ID [!DNL Generic REST API]. この固定 ID は次のとおりです。 `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62`. |
| `auth.specName` | Platform へのソースの認証に使用する認証タイプ。 |
| `auth.params.host` | の [!DNL Generic REST API] ソース。 |
| `auth.params.accessToken` | ソースの認証に使用される、対応するアクセストークン。 これは、OAuth ベースの認証に必要です。 |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
  "id": "a5c6b647-e784-4b58-86b6-47e784ab580b",
  "etag": "\"7b01056a-0000-0200-0000-5e8a4f5b0000\""
}
```

### の作成 [!DNL Generic REST API] 基本認証を使用したベース接続

を作成するには [!DNL Generic REST API] 基本認証を使用したベース接続、 `/connections` 終点 [!DNL Flow Service] API を使用して基本認証の資格情報を指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Generic REST API]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `name` | ベース接続の名前。 ベース接続の名前がわかりやすいことを確認します。これを使用して、ベース接続の情報を検索できます。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | 関連付けられている接続仕様 ID [!DNL Generic REST API]. この固定 ID は次のとおりです。 `4e98f16f-87d6-4ef0-bdc6-7a2b0fe76e62`. |
| `auth.specName` | ソースを Platform に接続するために使用する認証タイプ。 |
| `auth.params.host` | の [!DNL Generic REST API] ソース。 |
| `auth.params.username` | の [!DNL Generic REST API] ソース。 これは基本認証に必要です。 |
| `auth.params.password` | の [!DNL Generic REST API] ソース。 これは基本認証に必要です。 |

**応答**

正常な応答は、新しく作成されたベース接続を返します。この中には、一意の接続識別子 (`id`) をクリックします。 この ID は、次の手順でソースのファイル構造とコンテンツを調べるために必要です。

```json
{
    "id": "9601747c-6874-4c02-bb00-5732a8c43086",
    "etag": "\"3702dabc-0000-0200-0000-615b5b5a0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Generic REST API] 接続 [!DNL Flow Service] API を持ち、接続の一意の ID 値を取得している。 この ID は、次のチュートリアルで、 [フローサービス API を使用したプロトコルアプリケーションの調査](../../explore/protocols.md).
