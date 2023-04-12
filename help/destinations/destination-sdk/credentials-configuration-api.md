---
description: このページでは、「/authoring/credentials」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 資格情報エンドポイント API の操作
exl-id: 89957f38-e7f4-452d-abc0-0940472103fe
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 91%

---

# 資格情報エンドポイント API の操作 {#credentials}

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、`/authoring/credentials` API エンドポイントを使用して実行できるすべての API の操作について説明します。

このエンドポイントでサポートされる機能については、以下を参照してください。

* ストリーミング宛先について設定できる機能の、[ストリーミング先の設定](destination-configuration.md)。
* ファイルベースの宛先について設定できる機能の、[ファイルベースの宛先設定](file-based-destination-configuration.md)。

## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、`/credentials` API エンドポイントを使用する必要は&#x200B;*ありません*。代わりに、`customerAuthenticationConfigurations` のパラメーター（`/destinations` エンドポイントにて）で認証情報を設定することができます。詳しくは、[認証設定](./authentication-configuration.md#when-to-use)をお読みください。

アドビと接続先との間にグローバル認証システムがある場合は、この API エンドポイントを使用し、[宛先設定](./destination-configuration.md#destination-delivery)で `PLATFORM_AUTHENTICATION` を選択します。[!DNL Platform] ユーザーは、接続先に接続するために認証資格情報を提供する必要はありません。この場合、`/credentials` API エンドポイントを使用して、認証情報オブジェクトを作成する必要があります。

## 認証情報設定 API の操作の基本を学ぶ {#get-started}

続行する前に、[入門ガイド](./getting-started.md)で、必要な宛先作成許可やヘッダーの取得方法など、API に対する呼び出しを正常に行うためにに知っておく必要がある、重要な情報を確認しておいてください。

## 認証情報設定の作成 {#create}

`/authoring/credentials` エンドポイントに POST リクエストを実行することで、新しい認証情報の構成を作成することができます。

**API 形式**

```http
POST /authoring/credentials
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターによって構成される、新しい認証情報の構成を作成します。以下のペイロードには、`/authoring/credentials` エンドポイントで使用できるすべてのパラメータを含みます。 呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートは API 要件に応じてカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "oauth2UserAuthentication":{
      "url":"string",
      "clientId":"string",
      "clientSecret":"string",
      "username":"string",
      "password":"string",
      "header":"string"
   },
   "oauth2ClientAuthentication":{
      "url":"string",
      "clientId":"string",
      "clientSecret":"string",
      "header":"string",
      "developerToken":"string"
   },
   "oauth2AccessTokenAuthentication":{
      "accessToken":"string",
      "expiration":"string",
      "username":"string",
      "userId":"string",
      "url":"string",
      "header":"string"
   },
   "oauth2RefreshTokenAuthentication":{
      "refreshToken":"string",
      "expiration":"string",
      "clientId":"string",
      "clientSecret":"string",
      "url":"string",
      "header":"string"
   },
   "s3Authentication":{
      "accessId":"string",
      "secretKey":"string"
   },
   "sshAuthentication":{
      "username":"string",
      "sshKey":"string"
   },
   "azureAuthentication":{
      "url":"string",
      "tenant":"string",
      "servicePrincipalId":"string",
      "servicePrincipalKey":"string"
   },
   "azureConnectionStringAuthentication":{
      "connectionString":"string"
   },
   "basicAuthentication":{
      "url":"string",
      "username":"string",
      "password":"string"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `username` | 文字列 | 認証情報設定ログインユーザー名 |
| `password` | 文字列 | 認証情報構成のログインパスワード |
| `url` | 文字列 | 認証プロバイダーの URL |
| `clientId` | 文字列 | クライアント／アプリケーション認証情報のクライアント ID |
| `clientSecret` | 文字列 | クライアント／アプリケーション認証情報のクライアント秘密鍵 |
| `accessToken` | 文字列 | 認証プロバイダーから提供されたアクセストークン |
| `expiration` | 文字列 | アクセストークンの有効期間 |
| `refreshToken` | 文字列 | 認証プロバイダーから提供された更新トークン |
| `header` | 文字列 | 認証に必要なヘッダー |
| `accessId` | 文字列 | Amazon S3 アクセス ID |
| `secretKey` | 文字列 | Amazon S3 秘密鍵 |
| `sshKey` | 文字列 | SSH 認証を使用した SFTP 用の SSH キー |
| `tenant` | 文字列 | Azure Data Lake Storage のテナント |
| `servicePrincipalId` | 文字列 | Azure Data Lake Storage の Azure サービスプリンシパル ID |
| `servicePrincipalKey` | 文字列 | Azure Data Lake Storage の Azure サービスプリンシパルキー |
| `connectionString` | 文字列 | Azure Blob ストレージ接続文字列 |

{style="table-layout:auto"}

**応答**

リクエストが成功した場合は、新しく作成した資格情報の構成の詳細とともに、HTTP ステータス 200 が返されます。

## 資格情報の設定リスト {#retrieve-list}

組織のすべての資格情報設定のリストを取得するには、 `/authoring/credentials` endpoint.

**API 形式**


```http
GET /authoring/credentials
```

**リクエスト**

次のリクエストは、組織とサンドボックスの設定に基づいて、アクセス権のある資格情報設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

次の応答は、使用した組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある資格情報設定のリストを返します。 1 つの `instanceId` は、1 つの資格情報設定のテンプレートに対応します。簡潔にするために、応答は切り捨てられます。

```json
{
   "items":[
      {
         "instanceId":"n55affa0-3747-4030-895d-1d1236bb3680",
         "createdDate":"2021-06-07T06:41:48.641943Z",
         "lastModifiedDate":"2021-06-07T06:41:48.641943Z",
         "type":"OAUTH2_USER_CREDENTIAL",
         "name":"yourdestination",
         "oauth2UserAuthentication":{
            "url":"ABCD",
            "clientId":"ABCDEFGHIJKL",
            "clientSecret":"clientsecret",
            "username":"username",
            "password":"password",
            "header":"header"
         }
      }
   ]
}
    
```

## 既存の資格情報設定の更新 {#update}

既存の資格情報設定を更新するには、エンドポイント `/authoring/credentials` に PUT リクエストを行い、更新する資格情報設定のインスタンス ID を指定します。呼び出しの本文で、更新された資格情報設定を指定します。

**API 形式**


```http
PUT /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する資格情報設定の ID。 |

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、既存の資格情報の設定を更新します。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "instanceId":"n55affa0-3747-4030-895d-1d1236bb3680",
   "createdDate":"2021-06-07T06:41:48.641943Z",
   "lastModifiedDate":"2021-06-07T06:41:48.641943Z",
   "type":"OAUTH2_USER_CREDENTIAL",
   "name":"yourdestination",
   "oauth2UserAuthentication":{
      "url":"ABCD",
      "clientId":"ABCDEFGHIJKL",
      "clientSecret":"clientsecret",
      "username":"username",
      "password":"password",
      "header":"header"
   }
}
```

## 特定の資格情報設定の取得 {#get}

特定の資格情報設定に関する詳細な情報を取得するには、エンドポイント `/authoring/credentials` に GET リクエストを実行し、更新する資格情報設定のインスタンス ID を指定します。

**API 形式**

```http
GET /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する資格情報設定の ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、指定された資格情報設定と共に HTTP ステータス 200 が返されます。

```json
{
   "instanceId":"n55affa0-3747-4030-895d-1d1236bb3680",
   "createdDate":"2021-06-07T06:41:48.641943Z",
   "lastModifiedDate":"2021-06-07T06:41:48.641943Z",
   "type":"OAUTH2_USER_CREDENTIAL",
   "name":"yourdestination",
   "oauth2UserAuthentication":{
      "url":"ABCD",
      "clientId":"ABCDEFGHIJKL",
      "clientSecret":"clientsecret",
      "username":"username",
      "password":"password",
      "header":"header"
   }
}
```

## 特定の資格情報設定の削除 {#delete}

指定した資格情報設定を削除するには、エンドポイント `/authoring/credentials` に DELETE リクエストを実行し、リクエストパスで削除する資格情報設定の ID を指定します。

**API 形式**

```http
DELETE /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する資格情報設定の `id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、資格情報エンドポイントを使用するタイミングと、`/authoring/credentials` API エンドポイントまたは `/authoring/destinations` エンドポイントを使用して資格情報設定を設定する方法を確認しました。[Destination SDK を使用して宛先を設定する方法](./configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。
