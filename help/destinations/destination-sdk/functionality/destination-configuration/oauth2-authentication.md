---
description: このページでは、Destination SDK でサポートされている様々な OAuth 2 認証フローについて説明し、宛先用の OAuth 2 認証の設定手順を示します。
title: OAuth 2 認証
exl-id: 280ecb63-5739-491c-b539-3c62bd74e433
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '2155'
ht-degree: 100%

---

# OAuth 2 認証

Destination SDK は、宛先に対して、いくつかの認証方法をサポートしています。その中に、[OAuth 2 認証フレームワーク](https://tools.ietf.org/html/rfc6749)を使用して宛先を認証するオプションがあります。

このページでは、Destination SDK でサポートされている様々な OAuth 2 認証フローについて説明し、宛先用の OAuth 2 認証の設定手順を示します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | × |

## OAuth 2 認証の詳細の宛先設定への追加方法 {#how-to-setup}

### システムの前提条件 {#prerequisites}

最初の手順として、お使いのシステムで Adobe Experience Platform 用にアプリを作成するか、またはお使いのシステムで Experience Platform を登録する必要があります。目的は、宛先に対して Experience Platform を認証するために必要な、クライアント ID およびクライアントシークレットを生成することです。お使いのシステムでのこの設定の一環として、Adobe Experience Platform OAuth 2 リダイレクト／コールバック URL が必要です（以下のリストから取得できます）。

* `https://platform-va7.adobe.io/data/core/activation/oauth/api/v1/callback`
* `https://platform-nld2.adobe.io/data/core/activation/oauth/api/v1/callback`
* `https://platform-aus5.adobe.io/data/core/activation/oauth/api/v1/callback`

>[!IMPORTANT]
>
>お使いのシステムで Adobe Experience Platform 用にリダイレクト／コールバック URL を登録する手順は、[認証コード付与タイプを含む OAuth 2](oauth2-authentication.md#authorization-code) の場合にのみ必須です。その他の 2 つのサポートされる付与タイプ（パスワードとクライアント資格情報）の場合、この手順をスキップできます。

この手順の終了時に、以下を持っている必要があります。
* クライアント ID
* クライアントシークレット
* アドビのコールバック URL（認証コード付与用）。

### Destination SDK で行う必要があること {#to-do-in-destination-sdk}

Experience Platform で宛先用に OAuth 2 認証を設定するには、`customerAuthenticationConfigurations` パラメーターの[宛先設定](../../authoring-api/destination-configuration/create-destination-configuration.md)に OAuth 2 の詳細を追加する必要があります。詳細な例については、[顧客認証](../../functionality/destination-configuration/customer-authentication.md)を参照してください。OAuth 2 認証付与タイプに応じて、設定テンプレートに追加する必要があるフィールドに関する具体的な手順については、このページで後述します。

## サポートされる OAuth 2 付与タイプ {#oauth2-grant-types}

Experience Platform は、以下の表にある 3 つの OAuth 2 付与タイプをサポートします。カスタム OAuth 2 設定がある場合、アドビは、統合のカスタムフィールドを活用して、それをサポートできます。詳しくは、付与タイプに関する各節を参照してください。

>[!IMPORTANT]
>
>* 以下の節で指示されているように、入力パラメーターを指定します。アドビの内部システムは、プラットフォームの認証システムに接続して、出力パラメーターを取得します（ユーザーを認証したり、宛先への認証を維持したりするために使用されます）。
>* 表内で太字でハイライト表示されている入力パラメーターは、OAuth 2 認証フローで必須のパラメーターです。その他のパラメーターはオプションです。ここに示されていないその他のカスタム入力パラメーターについては、[OAuth 2 設定のカスタマイズ](#customize-configuration)および[アクセストークンの更新](#access-token-refresh)の節で詳しく説明しています。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| 認証コード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>authorizationUrl</b></li><li><b>accessTokenUrl</b></li><li>refreshTokenUrl</li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |
| パスワード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li><li><b>username</b></li><li><b>password</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |
| クライアント資格情報 | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

上記の表には、標準的な OAuth 2 フローで使用されるフィールドが示されています。これらの標準的なフィールドに加えて、様々なパートナー統合では、追加の入力および出力が必要になる可能性があります。アドビでは、Destination SDK 用に柔軟な OAuth 2 認証／認証フレームワークを設計しています。上記の標準的なフィールドパターンに対するバリエーションを処理できると同時に、無効な出力（期限切れのアクセストークンなど）を自動的に再生成するメカニズムをサポートしています。

どのような場合でも、出力には、アクセストークン（Experience Platform が宛先に対して認証したり、認証を維持したりするのに使用）が含まれています。

OAuth 2 認証のためにアドビが設計したシステムは、以下をサポートしています。
* 3 つの OAuth 2 付与をすべてサポートすると同時に、それらのバリエーション（追加のデータフィールド、非標準的な API 呼び出しなど）を考慮します。
* 90 日、30 分または指定したその他のライフタイム値など、様々なライフタイム値を持つアクセストークンをサポートします。
* 更新トークンを含む／含まない OAuth 2 認証フローをサポートします。

## 認証コードを使用した OAuth 2 {#authorization-code}

宛先が標準的な OAuth 2.0 認証コードフロー（[RFC 標準仕様](https://tools.ietf.org/html/rfc6749#section-4.1)を参照）またはそのバリエーションをサポートしている場合は、以下の必須フィールドおよびオプションフィールドを参照してください。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| 認証コード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>authorizationUrl</b></li><li><b>accessTokenUrl</b></li><li>refreshTokenUrl</li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

宛先に対してこの認証方法を設定するには、[宛先設定を作成](../../authoring-api/destination-configuration/create-destination-configuration.md)する際に、設定に以下の行を追加します。

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
| `grant` | 文字列 | 「OAUTH2_AUTHORIZATION_CODE」を使用します。 |
| `accessTokenUrl` | 文字列 | アクセストークンと（オプションで）更新トークンを発行する、お客様側の URL。 |
| `authorizationUrl` | 文字列 | アプリケーションにログインするためにユーザーをリダイレクトさせる、認証サーバーの URL。 |
| `refreshTokenUrl` | 文字列 | *オプション。* 更新トークンを発行する、お客様側の URL。多くの場合、`refreshTokenUrl` は、`accessTokenUrl` と同じです。 |
| `clientId` | 文字列 | システムが Adobe Experience Platform に割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムが Adobe Experience Platform に割り当てるクライアントシークレット。 |
| `scope` | 文字列のリスト | *オプション*。アクセストークンが Experience Platform に対してリソース上で実行を許可する範囲を設定します。例：&quot;read, write&quot;。 |

{style="table-layout:auto"}

## パスワード付与を使用した OAuth 2

OAuth 2 パスワード付与（[RFC 標準仕様](https://tools.ietf.org/html/rfc6749#section-4.3)を参照）の場合、Experience Platform には、ユーザーのユーザー名とパスワードが必要です。認証フローでは、Experience Platform は、これらの資格情報をアクセストークンおよび（オプションで）更新トークンと交換します。
アドビでは、宛先設定をシンプル化するために、値を上書きする機能を備えた、以下の標準入力を利用します。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| パスワード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li><li><b>username</b></li><li><b>password</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

>[!NOTE]
>
> 以下の設定の `username` および `password` に対して、任意のパラメーターを追加する必要はありません。宛先設定で `"grant": "OAUTH2_PASSWORD"` を追加すると、システムは、宛先を認証する際に、ユーザーに Experience Platform UI でユーザー名およびパスワードを指定することをリクエストします。

宛先に対してこの認証方法を設定するには、[宛先設定を作成](../../authoring-api/destination-configuration/create-destination-configuration.md)する際に、設定に以下の行を追加します。

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
| `accessTokenUrl` | 文字列 | アクセストークンと（オプションで）更新トークンを発行する、お客様側の URL。 |
| `clientId` | 文字列 | システムが Adobe Experience Platform に割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムが Adobe Experience Platform に割り当てるクライアントシークレット。 |
| `scope` | 文字列のリスト | *オプション*。アクセストークンが Experience Platform に対してリソース上で実行を許可する範囲を設定します。例：&quot;read, write&quot;。 |

{style="table-layout:auto"}

## クライアント資格情報付与を使用した OAuth 2

以下に示す標準入力および出力をサポートする、OAuth 2 クライアント資格情報（[RFC 標準仕様](https://tools.ietf.org/html/rfc6749#section-4.4)を参照）宛先を設定できます。値をカスタマイズできます。詳しくは、[OAuth 2 設定のカスタマイズ](#customize-configuration)を参照してください。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| クライアント資格情報 | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

宛先に対してこの認証方法を設定するには、[宛先設定を作成](../../authoring-api/destination-configuration/create-destination-configuration.md)する際に、設定に以下の行を追加します。

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
| `accessTokenUrl` | 文字列 | アクセストークンと（オプションで）更新トークンを発行する、認証サーバーの URL。 |
| `refreshTokenUrl` | 文字列 | *オプション。* 更新トークンを発行する、お客様側の URL。多くの場合、`refreshTokenUrl` は、`accessTokenUrl` と同じです。 |
| `clientId` | 文字列 | システムが Adobe Experience Platform に割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムが Adobe Experience Platform に割り当てるクライアントシークレット。 |
| `scope` | 文字列のリスト | *オプション*。アクセストークンが Experience Platform に対してリソース上で実行を許可する範囲を設定します。例：&quot;read, write&quot;。 |

{style="table-layout:auto"}

## OAuth 2 設定のカスタマイズ {#customize-configuration}

上記の節で説明されている設定は、標準的な OAuth 2 付与を記述しています。ただし、アドビが設計したシステムには、OAuth 2 付与のバリエーションにカスタムパラメーターを使用できる柔軟性があります。標準的な OAuth 2 設定をカスタマイズするには、以下の例に示すように、`authenticationDataFields` パラメーターを使用します。

### 例 1：`authenticationDataFields` を使用した認証応答に由来する情報のキャプチャ {#example-1}

この例では、宛先プラットフォームには、一定期間後に期限切れになる更新トークンがあります。この場合、パートナーは、`refreshTokenExpiration` カスタムフィールドを設定して、API 応答の `refresh_token_expires_in` フィールドから更新トークンの有効期限を取得します。

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

### 例 2：`authenticationDataFields` を使用した特別な更新トークンの提供 {#example-2}

この例では、パートナーは、特別な更新トークンを提供するように宛先を設定します。さらに、アクセストークンの有効期限は、API 応答では返されないので、デフォルト値をハードコーディングできます（この場合は 3600 秒）。

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

### 例 3：宛先を設定する際のクライアント ID およびクライアントシークレットのユーザー入力 {#example-3}

この例では、[システムの前提条件](#prerequisites)の節に示すようにグローバルクライアント ID およびクライアントシークレットを作成する代わりに、顧客が、クライアント ID、クライアントシークレットおよびアカウント ID（顧客が宛先にログインするために使用する ID）を入力する必要があります。

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
                    "source": "CUSTOMER"
                },
                {
                    "name": "clientSecret",
                    "title": "Client Secret",
                    "description": "Client Secret",
                    "type": "string",
                    "isRequired": true,
                    "format": "password",
                    "source": "CUSTOMER"
                },
                {
                    "name": "moviestarId",
                    "title": "Moviestar ID",
                    "description": "Moviestar ID",
                    "type": "string",
                    "isRequired": true,
                    "source": "CUSTOMER"
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



`authenticationDataFields` で以下のパラメーターを使用して、OAuth 2 設定をカスタマイズできます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationDataFields.name` | 文字列 | カスタムフィールドの名前。 |
| `authenticationDataFields.title` | 文字列 | カスタムフィールドに対して指定できるタイトル。 |
| `authenticationDataFields.description` | 文字列 | 設定するカスタムデータフィールドの説明。 |
| `authenticationDataFields.type` | 文字列 | カスタムデータフィールドのタイプを定義します。<br> 使用できる値：`string`、`boolean`、`integer` |
| `authenticationDataFields.isRequired` | ブール値 | 認証フローでカスタムデータフィールドが必須かどうかを指定します。 |
| `authenticationDataFields.format` | 文字列 | `"format":"password"` を選択すると、アドビは、認証データフィールドの値を暗号化します。`"fieldType": "CUSTOMER"` と共に使用された場合、これも、ユーザーがフィールドに入力する際に UI で入力を非表示にします。 |
| `authenticationDataFields.fieldType` | 文字列 | Experience Platform で宛先を設定する際に、入力がパートナー（あなた）かユーザーのどちらによるものかを示します。 |
| `authenticationDataFields.value` | 文字列.ブール値.整数 | カスタムデータフィールドの値 。値は、`authenticationDataFields.type` から選択したタイプに一致します。 |
| `authenticationDataFields.authenticationResponsePath` | 文字列 | API 応答パスのどのフィールドを参照しているかを示します。 |

{style="table-layout:auto"}

## アクセストークンの更新 {#access-token-refresh}

アドビは、ユーザーがプラットフォームにログインし直す必要なしに、期限切れのアクセストークンを更新するシステムを設計しています。このシステムは、新しいトークンを生成できるので、宛先に対するアクティベーションは、お客様にとってシームレスに継続されます。

アクセストークンの更新を設定するには、更新トークンを使用して、アドビが新しいアクセストークンを取得できる、テンプレート化された HTTP リクエストを設定する必要がある可能性があります。アクセストークンの有効期限が切れている場合、アドビは、お客様から提供されたテンプレート化されたリクエストを受け取り、指定されたパラメーターを追加します。`accessTokenRequest` パラメーターを使用して、アクセストークンの更新メカニズムを設定します。


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

`accessTokenRequest` で以下のパラメーターを使用して、トークン更新プロセスをカスタマイズできます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `accessTokenRequest.destinationServerType` | 文字列 | `URL_BASED`.を使用します。 |
| `accessTokenRequest.urlBasedDestination.url.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.urlBasedDestination.url.value` の値に対してテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.urlBasedDestination.url.value` の値が定数である場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.urlBasedDestination.url.value` | 文字列 | Experience Platform がアクセストークンをリクエストする URL。 |
| `accessTokenRequest.httpTemplate.requestBody.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.httpTemplate.requestBody.value` の値に対してテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.httpTemplate.requestBody.value` の値が定数である場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.httpTemplate.requestBody.value` | 文字列 | テンプレート言語を使用して、アクセストークンエンドポイントに対する HTTP リクエストのフィールドをカスタマイズします。テンプレートを使用したフィールドのカスタマイズ方法について詳しくは、[テンプレート規則](#templating-conventions)の節を参照してください。 |
| `accessTokenRequest.httpTemplate.httpMethod` | 文字列 | アクセストークンエンドポイントの呼び出しに使用された HTTP メソッドを指定します。ほとんどの場合、この値は、`POST` です。 |
| `accessTokenRequest.httpTemplate.contentType` | 文字列 | アクセストークンエンドポイントに対する HTTP 呼び出しのコンテンツタイプを指定します。<br> 例：`application/x-www-form-urlencoded` または `application/json`。 |
| `accessTokenRequest.httpTemplate.headers` | 文字列 | アクセストークンエンドポイントに対する HTTP 呼び出しに任意のヘッダーを追加する必要があるかどうかを指定します。 |
| `accessTokenRequest.responseFields.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.responseFields.value` の値に対してテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.responseFields.value` の値が定数である場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.responseFields.value` | 文字列 | テンプレート言語を使用して、アクセストークンエンドポイントからの HTTP 応答のフィールドにアクセスします。テンプレートを使用したフィールドのカスタマイズ方法について詳しくは、[テンプレート規則](#templating-conventions)の節を参照してください。 |
| `accessTokenRequest.validations.name` | 文字列 | この検証に対して指定された名前を示します。 |
| `accessTokenRequest.validations.actualValue.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.validations.actualValue.value` の値に対してテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.validations.actualValue.value` の値が定数である場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.validations.actualValue.value` | 文字列 | テンプレート言語を使用して、HTTP 応答のフィールドにアクセスします。テンプレートを使用したフィールドのカスタマイズ方法について詳しくは、[テンプレート規則](#templating-conventions)の節を参照してください。 |
| `accessTokenRequest.validations.expectedValue.templatingStrategy` | 文字列 | <ul><li>`accessTokenRequest.validations.expectedValue.value` の値に対してテンプレートを使用する場合は、`PEBBLE_V1` を使用します。</li><li> フィールド `accessTokenRequest.validations.expectedValue.value` の値が定数である場合は、`NONE` を使用します。 </li></li> |
| `accessTokenRequest.validations.expectedValue.value` | 文字列 | テンプレート言語を使用して、HTTP 応答のフィールドにアクセスします。テンプレートを使用したフィールドのカスタマイズ方法について詳しくは、[テンプレート規則](#templating-conventions)の節を参照してください。 |

{style="table-layout:auto"}

## テンプレート規則 {#templating-conventions}

前の節で示したように、認証のカスタマイズに応じて、認証応答のデータフィールドにアクセスする必要がある可能性があります。これを行うために、アドビが使用する [Pebble テンプレート言語](https://pebbletemplates.io/)についてよく理解し、OAuth 2 実装をカスタマイズするための以下のテンプレート規則を参照してください。


| プレフィックス | 説明 | 例 |
|---------|----------|---------|
| authData | 任意のパートナーまたは顧客データフィールドの値にアクセスします。 | ``{{ authData.accessToken }}`` |
| response.body | HTTP 応答の本文 | ``{{ response.body.access_token }}`` |
| response.status | HTTP 応答ステータス | ``{{ response.status }}`` |
| response.headers | HTTP 応答ヘッダー | ``{{ response.headers.server[0] }}`` |
| userContext | 現在の認証試行に関する情報にアクセスします | <ul><li>`{{ userContext.sandboxName }} `</li><li>`{{ userContext.sandboxId }} `</li><li>`{{ userContext.imsOrgId }} `</li><li>`{{ userContext.client }} // the client executing the authentication attempt `</li></ul> |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むことで、Adobe Experience Platform でサポートされている OAuth 2 認証パターンを理解し、OAuth 2 認証サポートによる宛先の設定方法を知ることができました。次に、Destination SDK を使用して、OAuth 2 をサポートする宛先を設定できます。次の手順については、[Destination SDK を使用した宛先の設定](../../guides/configure-destination-instructions.md)を参照してください。
