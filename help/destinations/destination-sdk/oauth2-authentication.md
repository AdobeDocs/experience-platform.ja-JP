---
description: このページでは、宛先 SDK でサポートされる様々な OAuth 2 認証フローについて説明し、宛先の OAuth 2 認証を設定する手順を説明します。
title: OAuth 2 認証
exl-id: 280ecb63-5739-491c-b539-3c62bd74e433
source-git-commit: e8625d6de7707b3a159f95d4471a73cbbed25d21
workflow-type: tm+mt
source-wordcount: '2110'
ht-degree: 6%

---

# OAuth 2 認証

## 概要 {#overview}

宛先 SDK を使用して、Adobe Experience Platformが [OAuth 2 認証フレームワーク ](https://tools.ietf.org/html/rfc6749) を使用して宛先に接続できるようにします。

このページでは、宛先 SDK でサポートされる様々な OAuth 2 認証フローについて説明し、宛先の OAuth 2 認証を設定する手順を説明します。

## OAuth 2 認証の詳細を宛先設定に追加する方法 {#how-to-setup}

### システムの前提条件 {#prerequisites}

最初の手順として、Adobe Experience Platformのシステムでアプリを作成するか、システムにExperience Platformを登録する必要があります。 目的は、宛先へのExperience Platformを認証するために必要なクライアント ID とクライアントの秘密鍵を生成することです。 お使いのシステムでのこの設定の一環として、Adobe Experience Platform OAuth 2 リダイレクト/コールバック URL が必要です。これは、以下のリストから取得できます。

* `https://platform-va7.adobe.io/data/core/activation/oauth/api/v1/callback`
* `https://platform-nld2.adobe.io/data/core/activation/oauth/api/v1/callback`
* `https://platform-aus5.adobe.io/data/core/activation/oauth/api/v1/callback`

>[!IMPORTANT]
>
>システムにAdobe Experience Platformのリダイレクト/コールバック URL を登録する手順は、承認コード ](./oauth2-authentication.md#authorization-code) 付与タイプの [OAuth 2 に対してのみ必要です。 サポートされている他の 2 種類の付与タイプ（パスワードとクライアント資格情報）については、この手順をスキップできます。

この手順の最後には、次の内容が必要になります。
* クライアント ID
* クライアントの秘密鍵
* Adobeのコールバック URL（認証コード付与用）。

### 宛先 SDK で必要な操作 {#to-do-in-destination-sdk}

Experience Platformで宛先の OAuth 2 認証を設定するには、 `platform.adobe.io/data/core/activation/authoring/destinations` [API エンドポイント ](./destination-configuration-api.md) を使用して、 `customerAuthenticationConfigurations` パラメーターの下の [ 宛先設定 ](./destination-configuration.md) に OAuth 2 の詳細を追加する必要があります。 [ 設定例 ](./destination-configuration.md#example-configuration) を参照してください。 OAuth 2 認証付与タイプに応じて、設定テンプレートに追加する必要があるフィールドに関する具体的な手順を、このページで詳しく説明します。

## サポートされている OAuth 2 付与タイプ {#oauth2-grant-types}

Experience Platformは、次の表の 3 つの OAuth 2 付与タイプをサポートしています。 カスタム OAuth 2 設定がある場合、Adobeは、統合のカスタムフィールドを使用してそれをサポートできます。 詳細は、各付与タイプのセクションを参照してください。

>[!IMPORTANT]
>
>* 以下の節の説明に従って、入力パラメーターを指定します。 Adobe内部システムは、プラットフォームの認証システムに接続し、出力パラメーターを取得します。これは、ユーザーの認証と宛先への認証の維持に使用されます。
>* この表で太字で示されている入力パラメーターは、OAuth 2 認証フローで必要なパラメーターです。 その他のパラメーターはオプションです。 ここには示されていない他のカスタム入力パラメーターがありますが、詳しくは [OAuth 2 設定のカスタマイズ ](./oauth2-authentication.md#customize-configuration) と [ アクセストークンの更新 ](./oauth2-authentication.md#access-token-refresh) の節で説明しています。


| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| 認証コード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>範囲</li><li><b>authorizationUrl</b></li><li><b>accessTokenUrl</b></li><li>refreshTokenUrl</li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |
| パスワード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>範囲</li><li><b>accessTokenUrl</b></li><li><b>ユーザー</b></li><li><b>パスワード</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |
| クライアント資格情報 | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>範囲</li><li><b>accessTokenUrl</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style=&quot;table-layout:auto&quot;}

上記の表は、標準の OAuth 2 フローで使用されるフィールドの一覧です。 これらの標準フィールドに加えて、様々なパートナー統合で追加の入力および出力が必要になる場合があります。 Adobeは、宛先 SDK 用の柔軟な OAuth 2 認証/承認フレームワークを設計し、上記の標準フィールドパターンの変化に対応しながら、期限切れアクセストークンなどの無効な出力を自動的に再生成するメカニズムをサポートしています。

どのような場合でも、出力にはアクセストークンが含まれます。アクセストークンは、宛先への認証と管理をExperience Platformがおこなう際に使用されます。

Adobeが OAuth 2 認証用に設計したシステム：
* 追加のデータフィールド、非標準の API 呼び出しなど、3 つの OAuth 2 付与をすべてサポートし、それらのバリエーションを考慮します。
* 90 日間、30 分間、または指定したその他のライフタイム値に応じて、様々なライフタイム値を持つアクセストークンをサポートします。
* 更新トークンの有無に関わらず、OAuth 2 承認フローをサポートします。

## 認証コードを持つ OAuth 2 {#authorization-code}

宛先が標準の OAuth 2.0 認証コードフロー（[RFC 標準仕様 ](https://tools.ietf.org/html/rfc6749#section-4.1) を読む）またはそのバリエーションをサポートしている場合は、以下の必須フィールドとオプションフィールドを参照してください。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| 認証コード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>範囲</li><li><b>authorizationUrl</b></li><li><b>accessTokenUrl</b></li><li>refreshTokenUrl</li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style=&quot;table-layout:auto&quot;}

宛先にこの認証方法を設定するには、`/destinations` [endpoint](./destination-configuration.md) で次の行を設定に追加します。

```json
{
//...
  "customerAuthenticationConfigurations": [
    {
      "authType": "OAUTH2",
      "grant": "OAUTH2_AUTHORIZATION_CODE",
      "accessTokenUrl": "https://api.moviestar.com/OAuth/access_token",
      "authorizationUrl": "https://www.moviestar.com/dialog/OAuth",
      "refreshTokenUrl": "https://api.moviestar.com/OAuth/refresh_token",
      "clientId": "Experience-Platform-client-id",
      "clientSecret": "Experience-Platform-client-secret",
      "scope": ["read", "write"]
    }
  ]
//...
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authType` | 文字列 | 「OAUTH2」を使用します。 |
| `grant` | 文字列 | &quot;OAUTH2_AUTHORIZATION_CODE&quot;を使用します。 |
| `accessTokenUrl` | 文字列 | ユーザー側の URL。アクセストークンを発行し、必要に応じて更新トークンを発行します。 |
| `authorizationUrl` | 文字列 | ユーザーをアプリケーションにログインにリダイレクトする認証サーバーの URL。 |
| `refreshTokenUrl` | 文字列 | *オプション。* 側の URL で、更新トークンを発行します。`refreshTokenUrl` は `accessTokenUrl` と同じです。 |
| `clientId` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント秘密鍵。 |
| `scope` | 文字列のリスト | *オプション*. リソースに対してExperience Platformが実行できるアクセストークンの範囲を設定します。 例：「読み書き」 |

{style=&quot;table-layout:auto&quot;}

## OAuth 2（パスワード付与）

OAuth 2 パスワード付与（[RFC 標準仕様 ](https://tools.ietf.org/html/rfc6749#section-4.3) を読む）の場合、Experience Platformにはユーザーのユーザー名とパスワードが必要です。 認証フローで、Experience Platformはこれらの資格情報をアクセストークンと、オプションで更新トークンと交換します。
Adobeは、次の標準入力を利用して、宛先の設定を簡略化し、値を上書きできます。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| パスワード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>範囲</li><li><b>accessTokenUrl</b></li><li><b>ユーザー</b></li><li><b>パスワード</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
> `username` と `password` のパラメーターを以下の設定に追加する必要はありません。 宛先の設定に `"grant": "OAUTH2_PASSWORD"` を追加すると、ユーザーが宛先への認証を行う際に、Experience PlatformUI でのユーザー名とパスワードの入力が求められます。

宛先にこの認証方法を設定するには、`/destinations` [endpoint](./destination-configuration.md) で次の行を設定に追加します。

```json
{
//...
  "customerAuthenticationConfigurations": [
    {
      "authType": "OAUTH2",
      "grant": "OAUTH2_PASSWORD",
      "accessTokenUrl": "https://api.moviestar.com/OAuth/access_token",
      "clientId": "Experience-Platform-client-id",
      "clientSecret": "Experience-Platform-client-secret",
      "scope": ["read", "write"]
    }
  ]
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authType` | 文字列 | 「OAUTH2」を使用します。 |
| `grant` | 文字列 | 「OAUTH2_PASSWORD」を使用します。 |
| `accessTokenUrl` | 文字列 | ユーザー側の URL。アクセストークンを発行し、必要に応じて更新トークンを発行します。 |
| `clientId` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント秘密鍵。 |
| `scope` | 文字列のリスト | *オプション*. リソースに対してExperience Platformが実行できるアクセストークンの範囲を設定します。 例：「読み書き」 |

{style=&quot;table-layout:auto&quot;}

## クライアント資格情報付き OAuth 2 付与

OAuth 2 クライアント資格情報（[RFC 標準仕様 ](https://tools.ietf.org/html/rfc6749#section-4.4) を読み取り）の宛先を設定できます。この宛先は、以下に示す標準の入力と出力をサポートします。 値をカスタマイズできます。 詳しくは、[OAuth 2 設定のカスタマイズ ](./oauth2-authentication.md#customize-configuration) を参照してください。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| クライアント資格情報 | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>範囲</li><li><b>accessTokenUrl</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style=&quot;table-layout:auto&quot;}

宛先にこの認証方法を設定するには、`/destinations` [endpoint](./destination-configuration.md) で次の行を設定に追加します。

```json
{
//...
  "customerAuthenticationConfigurations": [
    {
      "authType": "OAUTH2",
      "grant": "OAUTH2_CLIENT_CREDENTIALS",
      "accessTokenUrl": "https://api.moviestar.com/OAuth/access_token",
      "refreshTokenUrl": "https://api.moviestar.com/OAuth/refresh_token",
      "clientId": "Experience-Platform-client-id",
      "clientSecret": "Experience-Platform-client-secret",
      "scope": ["read", "write"]
    }
  ]
//...
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authType` | 文字列 | 「OAUTH2」を使用します。 |
| `grant` | 文字列 | 「OAUTH2_CLIENT_CREDENTIALS」を使用します。 |
| `accessTokenUrl` | 文字列 | アクセストークンとオプションの更新トークンを発行する認証サーバーの URL。 |
| `refreshTokenUrl` | 文字列 | *オプション。* 側の URL で、更新トークンを発行します。`refreshTokenUrl` は `accessTokenUrl` と同じです。 |
| `clientId` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント秘密鍵。 |
| `scope` | 文字列のリスト | *オプション*. リソースに対してExperience Platformが実行できるアクセストークンの範囲を設定します。 例：「読み書き」 |

{style=&quot;table-layout:auto&quot;}

## OAuth 2 設定のカスタマイズ {#customize-configuration}

上記の節で説明した設定は、標準の OAuth 2 付与について説明します。 ただし、Adobeが設計したシステムは柔軟性を提供し、OAuth 2 付与の任意のバリエーションにカスタムパラメーターを使用できます。 標準の OAuth 2 設定をカスタマイズするには、次の例に示すように、`authenticationDataFields` パラメーターを使用します。

### 例 1:`authenticationDataFields` を使用して、認証応答から得られる情報を取り込む {#example-1}

この例では、宛先プラットフォームに、一定時間後に期限切れになる更新トークンがあります。 この場合、パートナーは `refreshTokenExpiration` カスタムフィールドを設定して、API 応答の `refresh_token_expires_in` フィールドから更新トークンの有効期限を取得します。

```json
{
   "customerAuthenticationConfigurations":[
      {
         "authType":"OAUTH2",
         "options":{
            
         },
         "grant":"OAUTH2_AUTHORIZATION_CODE",
         "accessTokenUrl":"https://api.moviestar.com/OAuth/access_token",
         "authorizationUrl":"https://api.moviestar.com/OAuth/authorization",
         "scope":[
            "read",
            "write",
            "delete"
         ],
         "refreshTokenUrl":"https://api.moviestar.com/OAuth/accessToken",
         "clientSecret":"client-secret-here",
         "authenticationDataFields":[
            {
               "name":"refreshTokenExpiration",
               "title":"Refresh Token Expires In",
               "description":"Time in seconds when the refresh token will expire",
               "type":"string",
               "isRequired":false,
               "source":"CUSTOMER",
               "authenticationResponsePath":"refresh_token_expires_in"
            }
         ]
      }
   ]
}  
```

### 例 2:`authenticationDataFields` を使用して特別な更新トークンを提供する {#example-2}

この例では、パートナーは、特別な更新トークンを提供するために宛先を設定します。 さらに、アクセストークンの有効期限は API 応答で返されないので、デフォルト値（この場合は 3600 秒）をハードコードできます。

```json
      "authenticationDataFields": [
        {
            "name": "refreshToken",
            "value": "special_refresh_token"
        },
        {
            "name": "expiresIn",
            "value": 3600
        } 
      ]
```

### 例 3:ユーザーは、宛先を設定する際に、クライアント ID とクライアントの秘密鍵を入力します {#example-3}

この例では、システムの前提条件 ](./oauth2-authentication.md#prerequisites) で示すように、グローバルクライアント ID とクライアント秘密鍵を作成する代わりに、クライアント ID、クライアント秘密鍵、アカウント ID（顧客が宛先にログインする際に使用する ID）を入力する必要があります[

```json
{
    //...
    "customerAuthenticationConfigurations": [
        {
            "authType": "OAUTH2",
            "grant": "OAUTH2_CLIENT_CREDENTIALS",
            "authenticationDataFields": [
                {
                    "name": "clientId",
                    "title": "Client ID",
                    "description": "Client ID",
                    "type": "string",
                    "isRequired": true,
                    "fieldType": "CUSTOMER"
                },
                {
                    "name": "clientSecret",
                    "title": "Client Secret",
                    "description": "Client Secret",
                    "type": "string",
                    "isRequired": true,
                    "format": "password",
                    "fieldType": "CUSTOMER"
                },
                {
                    "name": "moviestarId",
                    "title": "Moviestar ID",
                    "description": "Moviestar ID",
                    "type": "string",
                    "isRequired": true,
                    "fieldType": "CUSTOMER"
                }
            ],
            "accessTokenRequest": {
                "destinationServerType": "URL_BASED",
                "urlBasedDestination": {
                    "url": {
                        "templatingStrategy": "PEBBLE_V1",
                        "value": "https://{{ authData.moviestarId }}.yourdestination.com/identity/oauth/token"
                    }
                },
                "httpTemplate": {
                    "requestBody": {
                        "templatingStrategy": "PEBBLE_V1",
                        "value": "{{ formUrlEncode('grant_type', 'client_credentials', 'client_id', authData.clientId, 'client_secret', authData.clientSecret) | raw }}"
                    },
                    "httpMethod": "POST",
                    "contentType": "application/x-www-form-urlencoded"
                },
                "responseFields": [
                    {
                        "templatingStrategy": "PEBBLE_V1",
                        "value": "{{ response.body.access_token }}",
                        "name": "accessToken"
                    },
                    {
                        "templatingStrategy": "PEBBLE_V1",
                        "value": "{{ response.body.scope }}",
                        "name": "scope"
                    },
                    {
                        "templatingStrategy": "PEBBLE_V1",
                        "value": "{{ response.body.token_type }}",
                        "name": "tokenType"
                    },
                    {
                        "templatingStrategy": "PEBBLE_V1",
                        "value": "{{ response.body.expires_in }}",
                        "name": "expiresIn"
                    }
                ]
            }
        }
    ]
//...
}
```



`authenticationDataFields` で次のパラメーターを使用して、OAuth 2 の設定をカスタマイズできます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationDataFields.name` | 文字列 | カスタムフィールドの名前。 |
| `authenticationDataFields.title` | 文字列 | カスタムフィールドに指定できるタイトル。 |
| `authenticationDataFields.description` | 文字列 | 設定したカスタムデータフィールドの説明。 |
| `authenticationDataFields.type` | 文字列 | カスタムデータフィールドのタイプを定義します。 <br> 指定できる値： `string`,  `boolean`,  `integer` |
| `authenticationDataFields.isRequired` | Boolean | 認証フローでカスタムデータフィールドが必須かどうかを指定します。 |
| `authenticationDataFields.format` | 文字列 | `"format":"password"` を選択すると、Adobeは認証データフィールドの値を暗号化します。 `"fieldType": "CUSTOMER"` と共に使用すると、ユーザーがフィールドに入力したときに UI の入力も非表示になります。 |
| `authenticationDataFields.fieldType` | 文字列 | Experience Platformで宛先を設定した場合に、入力元がパートナー（自分）かユーザーかを示します。 |
| `authenticationDataFields.value` | 文字列. Boolean. 整数 | カスタムデータフィールドの値。 この値は、`authenticationDataFields.type` から選択したタイプと一致します。 |
| `authenticationDataFields.authenticationResponsePath` | 文字列 | 参照する API 応答パスのフィールドを示します。 |

{style=&quot;table-layout:auto&quot;}

## アクセストークンの更新 {#access-token-refresh}

Adobeは、ユーザーがプラットフォームにログインし直す必要なく、期限切れのアクセストークンを更新するシステムを設計しています。 システムは新しいトークンを生成できるので、宛先へのアクティベーションがお客様に対してシームレスに続行されます。

アクセストークンの更新を設定するには、テンプレート化された HTTP リクエストを設定し、Adobeが更新トークンを使用して新しいアクセストークンを取得できるようにする必要がある場合があります。 アクセストークンの有効期限が切れた場合、Adobeは指定されたテンプレートリクエストを受け取り、指定したパラメーターを追加します。 `accessTokenRequest` パラメーターを使用して、アクセストークンの更新メカニズムを設定します。


```json
{
   "customerAuthenticationConfigurations":[
      {
         "authType":"OAUTH2",
         "grant":"OAUTH2_CLIENT_CREDENTIALS",
         "accessTokenRequest":{
            "destinationServerType":"URL_BASED",
            "urlBasedDestination":{
               "url":{
                  "templatingStrategy":"PEBBLE_V1",
                  "value":"https://{{authData.customerId}}.yourdestination.com/identity/oauth/token"
               }
            },
            "httpTemplate":{
               "requestBody":{
                  "templatingStrategy":"PEBBLE_V1",
                  "value":"{{ formUrlEncode('grant_type', 'client_credentials', 'client_id', authData.clientId, 'client_secret', authData.clientSecret) | raw }}"
               },
               "httpMethod":"POST",
               "contentType":"application/x-www-form-urlencoded",
               "headers":[
                  
               ]
            },
            "responseFields":[
               {
                  "templatingStrategy":"PEBBLE_V1",
                  "value":"{{ response.body.expires_in }}",
                  "name":"expiresIn"
               },
               {
                  "templatingStrategy":"PEBBLE_V1",
                  "value":"{{ response.body.access_token }}",
                  "name":"accessToken"
               }
            ],
            "validations":[
               {
                  "name":"access_token validation",
                  "actualValue":{
                     "templatingStrategy":"PEBBLE_V1",
                     "value":"{{response.body.access_token is empty }}"
                  },
                  "expectedValue":{
                     "templatingStrategy":"PEBBLE_V1",
                     "value":"false"
                  }
               },
               {
                  "name":"response status",
                  "actualValue":{
                     "templatingStrategy":"PEBBLE_V1",
                     "value":"{{ response.status }}"
                  },
                  "expectedValue":{
                     "templatingStrategy":"PEBBLE_V1",
                     "value":"200"
                  }
               }
            ]
         }
      }
   ]
}
```

`accessTokenRequest` で次のパラメーターを使用して、トークンの更新プロセスをカスタマイズできます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `accessTokenRequest.destinationServerType` | 文字列 | `URL_BASED`.を使用します。 |
| `accessTokenRequest.urlBasedDestination.url.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.urlBasedDestination.url.value` の値にテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.urlBasedDestination.url.value` の値が定数の場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.urlBasedDestination.url.value` | 文字列 | Experience Platformがアクセストークンを要求する URL。 |
| `accessTokenRequest.httpTemplate.requestBody.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.httpTemplate.requestBody.value` の値にテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.httpTemplate.requestBody.value` の値が定数の場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.httpTemplate.requestBody.value` | 文字列 | テンプレート言語を使用して、アクセストークンエンドポイントへの HTTP リクエストのフィールドをカスタマイズします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、[ テンプレート規則 ](./oauth2-authentication.md#templating-conventions) の節を参照してください。 |
| `accessTokenRequest.httpTemplate.httpMethod` | 文字列 | アクセストークンエンドポイントの呼び出しに使用する HTTP メソッドを指定します。 ほとんどの場合、この値は `POST` です。 |
| `accessTokenRequest.httpTemplate.contentType` | 文字列 | アクセストークンエンドポイントへの HTTP 呼び出しのコンテンツタイプを指定します。 <br> 例： `application/x-www-form-urlencoded` または `application/json`。 |
| `accessTokenRequest.httpTemplate.headers` | 文字列 | アクセストークンエンドポイントへの HTTP 呼び出しにヘッダーを追加する必要があるかどうかを指定します。 |
| `accessTokenRequest.responseFields.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.responseFields.value` の値にテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.responseFields.value` の値が定数の場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.responseFields.value` | 文字列 | テンプレート言語を使用して、アクセストークンエンドポイントから HTTP 応答のフィールドにアクセスします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、[ テンプレート規則 ](./oauth2-authentication.md#templating-conventions) の節を参照してください。 |
| `accessTokenRequest.validations.name` | 文字列 | この検証に指定した名前を示します。 |
| `accessTokenRequest.validations.actualValue.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.validations.actualValue.value` の値にテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.validations.actualValue.value` の値が定数の場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.validations.actualValue.value` | 文字列 | テンプレート言語を使用して、HTTP 応答のフィールドにアクセスします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、[ テンプレート規則 ](./oauth2-authentication.md#templating-conventions) の節を参照してください。 |
| `accessTokenRequest.validations.expectedValue.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.validations.expectedValue.value` の値にテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.validations.expectedValue.value` の値が定数の場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.validations.expectedValue.value` | 文字列 | テンプレート言語を使用して、HTTP 応答のフィールドにアクセスします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、[ テンプレート規則 ](./oauth2-authentication.md#templating-conventions) の節を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## テンプレート規則 {#templating-conventions}

認証のカスタマイズに応じて、前の節で示したように、認証応答のデータフィールドにアクセスする必要が生じる場合があります。 そのためには、Adobeで使用される [Pebble templating language](https://pebbletemplates.io/) に慣れてから、以下のテンプレート規則を参照して OAuth 2 実装をカスタマイズしてください。


| プレフィックス | 説明 | 例 |
|---------|----------|---------|
| authData | 任意のパートナーまたは顧客データフィールドの値にアクセスする。 | ``{{ authData.accessToken }}`` |
| response.body | HTTP 応答本文 | ``{{ response.body.access_token }}`` |
| response.status | HTTP レスポンスステータス | ``{{ response.status }}`` |
| response.headers | HTTP 応答ヘッダー | ``{{ response.headers.server[0] }}`` |
| userContext | 現在の認証試行に関する情報にアクセス | <ul><li>`{{ userContext.sandboxName }} `</li><li>`{{ userContext.sandboxId }} `</li><li>`{{ userContext.imsOrgId }} `</li><li>`{{ userContext.client }} // the client executing the authentication attempt `</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 次の手順 {#next-steps}

この記事を読むと、Adobe Experience Platformでサポートされる OAuth 2 認証パターンと、OAuth 2 認証サポートを使用して宛先を設定する方法を理解できます。 次に、宛先 SDK を使用して、OAuth 2 対応の宛先を設定できます。 [ 宛先 SDK を使用して、次の手順で宛先 ](./configure-destination-instructions.md) を設定します。
