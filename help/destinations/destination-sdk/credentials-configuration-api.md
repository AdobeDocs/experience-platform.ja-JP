---
description: このページでは、「/authoring/credentials」 APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。
title: Credentials endpoint API操作
source-git-commit: 19307fba8f722babe5b6d57e80735ffde00fc851
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 6%

---

# Credentials endpoint API操作 {#credentials}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、`/authoring/credentials` APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。

## `/credentials` APIエンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、*APIエンドポイントを`/credentials`使用する必要は*&#x200B;ありません。 代わりに、`/destinations`エンドポイントの`customerAuthenticationConfigurations`パラメーターを使用して、宛先の認証情報を設定できます。 詳しくは、[資格情報の設定](./credentials-configuration.md)をお読みください。

Adobeと宛先の間にグローバル認証システムがあり、宛先に接続するための認証資格情報を[!DNL Platform]顧客が提供する必要がない場合は、このAPIエンドポイントを使用し、[宛先設定](./destination-configuration.md#destination-delivery)で`PLATFORM_AUTHENTICATION`を選択します。 この場合、`/credentials` APIエンドポイントを使用してcredentialsオブジェクトを作成する必要があります。

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

## 資格情報設定API操作の概要 {#get-started}

続行する前に、[はじめに](./getting-started.md)を参照し、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 資格情報設定の作成 {#create}

`/authoring/credentials`エンドポイントにPOSTリクエストを送信して、新しい資格情報の設定を作成できます。

**API 形式**


```http
POST /authoring/credentials
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターによって設定された、新しい資格情報設定を作成します。 以下のペイロードには、`/authoring/credentials`エンドポイントで受け入れられるすべてのパラメーターが含まれます。 APIの要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートがカスタマイズ可能であることに注意してください。

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
| `password` | 文字列 | 資格情報ログインパスワード |
| `url` | 文字列 | 認証プロバイダーのURL |
| `clientId` | 文字列 | クライアント/アプリケーション資格情報のクライアントID |
| `clientSecret` | 文字列 | クライアント/アプリケーション資格情報のクライアント秘密鍵 |
| `accessToken` | 文字列 | 認証プロバイダーから提供されたアクセストークン |
| `expiration` | 文字列 | アクセストークンの有効期間 |
| `refreshToken` | 文字列 | 認証プロバイダーから提供された更新トークン |
| `header` | 文字列 | 認証に必要なヘッダー |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTPステータス200と、新しく作成された資格情報設定の詳細を返します。

## 資格情報設定のリスト {#retrieve-list}

`/authoring/credentials`エンドポイントに対してGETリクエストを実行することで、IMS組織のすべての資格情報設定のリストを取得できます。

**API 形式**


```http
GET /authoring/credentials
```

**リクエスト**

次のリクエストは、IMS組織とサンドボックス設定に基づいて、アクセス権のある資格情報設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

次の応答は、使用したIMS組織IDとサンドボックス名に基づいて、HTTPステータス200と、アクセス権のある資格情報設定のリストを返します。 `instanceId`の1つは、1つの資格情報設定のテンプレートに対応します。 簡潔にするために応答は切り捨てられます。

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

既存の資格情報の設定を更新するには、`/authoring/credentials`エンドポイントにPUTリクエストを送信し、更新する資格情報設定のインスタンスIDを指定します。 呼び出しの本文で、更新された資格情報の設定を指定します。

**API 形式**


```http
PUT /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する資格情報設定のID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された既存の資格情報設定を更新します。

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





## 特定の資格情報の設定の取得 {#get}

特定の資格情報設定に関する詳細な情報を取得するには、`/authoring/credentials`エンドポイントにGETリクエストを送信し、更新する資格情報設定のインスタンスIDを指定します。

**API 形式**


```http
GET /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する資格情報設定のID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTPステータス200と、指定された資格情報設定に関する詳細情報を返します。

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

`/authoring/credentials`エンドポイントにDELETEリクエストを送信し、リクエストパスで削除する資格情報設定のIDを指定することで、指定した資格情報設定を削除できます。

**API 形式**

```http
DELETE /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する資格情報設定の`id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/credentials/n55affa0-3747-4030-895d-1d1236bb3680 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTPステータス200と空のHTTP応答を返します。

## APIエラー処理

宛先SDK APIエンドポイントは、一般的なExperience PlatformAPIエラーメッセージの原則に従います。 Platformトラブルシューティングガイドの[APIステータスコード](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes)および[リクエストヘッダーエラー](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読むと、資格情報エンドポイントを使用するタイミングと、`/authoring/credentials` APIエンドポイントまたは`/authoring/destinations`エンドポイントを使用して資格情報を設定する方法がわかります。 [宛先SDKを使用して宛先](./configure-destination-instructions.md)を設定する方法を読み、この手順が宛先の設定プロセスにどのように適合するかを理解してください。
