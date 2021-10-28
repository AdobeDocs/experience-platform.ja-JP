---
description: このページでは、「/authoring/credentials」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 資格情報エンドポイント API 操作
exl-id: 89957f38-e7f4-452d-abc0-0940472103fe
source-git-commit: 0bd57e226155ee68758466146b5d873dc4fdca29
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 6%

---

# 資格情報エンドポイント API 操作 {#credentials}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、 `/authoring/credentials` API エンドポイント。

## 使用するタイミング `/credentials` API エンドポイント {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、 *しない* 使用する必要がある `/credentials` API エンドポイント。 代わりに、 `customerAuthenticationConfigurations` のパラメーター `/destinations` endpoint. 読み取り [認証設定](./authentication-configuration.md#when-to-use) を参照してください。

この API エンドポイントを使用し、「 」を選択します。 `PLATFORM_AUTHENTICATION` 内 [宛先設定](./destination-configuration.md#destination-delivery) Adobeと宛先の間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 `/credentials` API エンドポイント。

<!--

Commenting out the example configurations

## Example configurations

**Example configuration for a Basic authentication credential configuration with username and password**

```json
{
  "type": "BASIC",
  "name": "YOUR_DESTINATION_NAME",
  "basicAuthentication": {
    "username": "YOUR_DESTINATION_SERVER_USERNAME",
    "password": "YOUR_DESTINATION_SERVER_PASSWORD"
  }
}

```

**Example configuration for an OAuth2 credential configuration**

```json

{
  "oauth2AccessTokenAuthentication": {
    "accessToken": "YOUR_DESTINATION_SERVER_ACCESS_TOKEN",
    "expiration": "YOUR_TOKEN_TIME_TO_LIVE",
    "username": "YOUR_DESTINATION_SERVER_USERNAME",
    "userId": "YOUR_DESTINATION_USER_ID",
    "url": "AUTHORIZATION_PROVIDER_URL",
    "header": "YOUR_AUTHORIZATION_HEADER"
  }
}

```

The sections below list out the necessary parameters for each authentication type. Let us know which authentication type your server uses and provide us with the relevant information for your server type.

## Basic authentication

|Parameter | Type | Description|
|---------|----------|------|
|`username` | String | credentials configuration login username |
|`password` | String | credentials configuration login password |



// commenting out this part as these types of authentication methods are not supported in phase one

### S3 authentication

|Parameter | Type | Description|
|---------|----------|------|
|accessId | String | credentials configuration S3 credential Access key ID |
|secretKey | String | credentials configuration S3 credential Secret key |

### SSH 

|Parameter | Type | Description|
|---------|----------|------|
|username | String | credentials configuration SSH username |
|SSHKey | String | credentials configuration SSH key |



## OAuth1

|Parameter | Type | Description|
|---------|----------|------|
|`apiKey` | String | A value used by the Destinations Service to identify itself to the Service Provider. |
|`apiSecret` | String | Secret used by the Destinations Service to establish ownership of the API key to the Service Provider. |
|`acccessToken` | String | A value used by the Destinations Service to gain access to the Protected Resources on behalf of the User |
|`tokenSecret` | String | A secret used by the Destinations Service to establish ownership of an access token. |

## OAuth2 user credentials

|Parameter | Type | Description|
|---------|----------|------|
|`clientId` | String | Client ID of Client/Application credential |
|`clientSecret` | String | Client secret of Client/Application credential |
|`username` | String | The user's username to log on to your platform. |
|`password` | String | The user's password to log on to your platform. |
|`url` | String | URL of authorization provider |
|`header` | String | Any header required for authorization |

## OAuth2 client credentials

|Parameter | Type | Description|
|---------|----------|------|
|`clientId` | String | Client ID of Client/Application credential |
|`clientSecret` | String | Client secret of Client/Application credential |
|`username`| String | URL of authorization provider |
|`password` | String | Any header required for authorization |

## OAuth2 access token

|Parameter | Type | Description|
|---------|----------|------|
|`accessToken` | String | Access token provided by the authorization provider |
|`expiration` | String | The time-to-live for the access token |
|`username` | String | The user's username to log on to your platform. |
|`userId` | String | The user's ID with your platform. |
|`url` | String | URL of authorization provider |
|`header` | String | Any header required for authorization |

## OAuth2 refresh token

|Parameter | Type | Description|
|---------|----------|------|
|`clientId` | String | Client ID of Client/Application credential |
|`clientSecret` | String | Client secret of Client/Application credential |
|`refreshToken` | String | Refresh token provided by the authorization provider |
|`url` | String | URL of authorization provider |
|`expiration` | String | The time-to-live for the refresh token |
|`header` | String | Any header required for authorization |

-->

## 資格情報設定 API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## 資格情報設定の作成 {#create}

新しい資格情報の設定を作成するには、 `/authoring/credentials` endpoint.

**API 形式**


```http
POST /authoring/credentials
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい資格情報設定を作成します。 以下のペイロードには、 `/authoring/credentials` endpoint. API 要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートをカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `username` | 文字列 | 資格情報ログインユーザー名 |
| `password` | 文字列 | 資格情報設定ログインパスワード |
| `url` | 文字列 | 認証プロバイダーの URL |
| `clientId` | 文字列 | クライアント/アプリケーション秘密鍵証明書のクライアント ID |
| `clientSecret` | 文字列 | クライアント/アプリケーション秘密鍵証明書のクライアント秘密鍵 |
| `accessToken` | 文字列 | 認証プロバイダーから提供されたアクセストークン |
| `expiration` | 文字列 | アクセストークンの有効期間 |
| `refreshToken` | 文字列 | 認証プロバイダーから提供された更新トークン |
| `header` | 文字列 | 認証に必要なヘッダー |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成された資格情報設定の詳細を返します。

## 資格情報設定のリスト {#retrieve-list}

IMS 組織のすべての資格情報設定のリストを取得するには、にGETリクエストをおこないます `/authoring/credentials` endpoint.

**API 形式**


```http
GET /authoring/credentials
```

**リクエスト**

次のリクエストは、IMS 組織とサンドボックス設定に基づいて、アクセス権のある資格情報設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある資格情報設定のリストを返します。 1 `instanceId` は、1 つの資格情報設定のテンプレートに対応します。 簡潔にするために、応答は切り捨てられます。

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

既存の資格情報設定を更新するには、 `/authoring/credentials` エンドポイントを作成し、更新する資格情報設定のインスタンス ID を指定します。 呼び出しの本文で、更新された資格情報の設定を指定します。

**API 形式**


```http
PUT /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する資格情報設定の ID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された、既存の資格情報設定を更新します。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

特定の資格情報設定に関する詳細な情報を取得するには、 `/authoring/credentials` エンドポイントを作成し、更新する資格情報設定のインスタンス ID を指定します。

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された資格情報設定に関する詳細情報を返します。

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

指定した資格情報設定を削除するには、 `/authoring/credentials` エンドポイントを作成し、リクエストパスで削除する資格情報設定の ID を指定します。

**API 形式**

```http
DELETE /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | この `id` を設定します。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と空の HTTP 応答を返します。

## API エラー処理

宛先 SDK API エンドポイントは、Experience PlatformAPI エラーメッセージの一般的な原則に従います。 参照： [API ステータスコード](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes) および [リクエストヘッダーエラー](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順

このドキュメントを読むと、資格情報エンドポイントを使用するタイミングと、 `/authoring/credentials` API エンドポイントまたは `/authoring/destinations` endpoint. 読み取り [宛先 SDK を使用して宛先を設定する方法](./configure-destination-instructions.md) を参照して、この手順が宛先を設定するプロセスに適した場所を把握します。
