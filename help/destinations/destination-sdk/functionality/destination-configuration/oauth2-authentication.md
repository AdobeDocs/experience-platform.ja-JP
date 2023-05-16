---
description: このページでは、Destination SDKがサポートする様々な OAuth 2 認証フローについて説明し、宛先の OAuth 2 認証を設定する手順を説明します。
title: OAuth 2 認証
exl-id: 280ecb63-5739-491c-b539-3c62bd74e433
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '2155'
ht-degree: 4%

---

# OAuth 2 認証

Destination SDKは、宛先に対して複数の認証方法をサポートします。 これらの中で、 [OAuth 2 認証フレームワーク](https://tools.ietf.org/html/rfc6749).

このページでは、Destination SDKがサポートする様々な OAuth 2 認証フローについて説明し、宛先の OAuth 2 認証を設定する手順を説明します。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | いいえ |

## OAuth 2 認証の詳細を宛先設定に追加する方法 {#how-to-setup}

### システムの前提条件 {#prerequisites}

最初の手順として、Adobe Experience Platform用のアプリをシステム内で作成するか、システムにExperience Platformを登録する必要があります。 目的は、宛先へのExperience Platformを認証するために必要な、クライアント ID とクライアント秘密鍵を生成することです。 システムでのこの設定の一環として、Adobe Experience Platform OAuth 2 リダイレクト/コールバック URL が必要です。これは、以下のリストから取得できます。

* `https://platform-va7.adobe.io/data/core/activation/oauth/api/v1/callback`
* `https://platform-nld2.adobe.io/data/core/activation/oauth/api/v1/callback`
* `https://platform-aus5.adobe.io/data/core/activation/oauth/api/v1/callback`

>[!IMPORTANT]
>
>システムにAdobe Experience Platformのリダイレクト/コールバック URL を登録する手順は、 [認証コードを持つ OAuth 2](oauth2-authentication.md#authorization-code) 付与タイプ。 サポートされている他の 2 つの付与タイプ（パスワードとクライアントの資格情報）については、この手順をスキップできます。

この手順の最後には、次の内容が必要になります。
* クライアント ID。
* クライアントの秘密鍵
* Adobeのコールバック URL （認証コード付与用）。

### Destination SDK {#to-do-in-destination-sdk}

Experience Platformで宛先の OAuth 2 認証を設定するには、OAuth 2 の詳細を [宛先設定](../../authoring-api/destination-configuration/create-destination-configuration.md)、 `customerAuthenticationConfigurations` パラメーター。 詳しくは、 [顧客認証](../../functionality/destination-configuration/customer-authentication.md) 詳細な例を参照してください。 OAuth 2 認証付与タイプに応じて、設定テンプレートに追加する必要があるフィールドに関する具体的な指示については、このページで詳しく説明します。

## サポートされている OAuth 2 付与タイプ {#oauth2-grant-types}

Experience Platformでは、次の表に示す 3 つの OAuth 2 付与タイプをサポートしています。 カスタム OAuth 2 設定がある場合、Adobeは、統合のカスタムフィールドを使用して、それをサポートできます。 詳細は、各付与タイプの節を参照してください。

>[!IMPORTANT]
>
>* 以下の節の説明に従って、入力パラメーターを指定します。 Adobe内部システムは、プラットフォームの認証システムに接続し、出力パラメーターを取得します。これは、ユーザーの認証と、宛先への認証の維持に使用されます。
>* この表で太字で示されている入力パラメーターは、OAuth 2 認証フローで必要なパラメーターです。 その他のパラメーターはオプションです。 ここには示されていない他のカスタム入力パラメータがありますが、詳しくは、の節で説明します [OAuth 2 設定のカスタマイズ](#customize-configuration) および [アクセストークンの更新](#access-token-refresh).


| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| 認証コード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>authorizationUrl</b></li><li><b>accessTokenUrl</b></li><li>refreshTokenUrl</li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |
| パスワード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li><li><b>ユーザー名</b></li><li><b>パスワード</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |
| クライアント資格情報 | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

上記の表は、標準の OAuth 2 フローで使用されるフィールドの一覧です。 これらの標準フィールドに加えて、様々なパートナー統合に対して、追加の入力および出力が必要になる場合があります。 Adobeは、上記の標準フィールドパターンのバリエーションに対応しながら、期限切れのアクセストークンなどの無効な出力を自動的に再生成するメカニズムをサポートできる、Destination SDK用の柔軟な OAuth 2 認証/承認フレームワークを設計しました。

どのような場合でも、出力にはアクセストークンが含まれます。アクセストークンは、宛先への認証と保守をExperience Platformがおこなうために使用されます。

Adobeが OAuth 2 認証用に設計したシステム：
* 追加のデータフィールド、非標準の API 呼び出しなど、3 つの OAuth 2 付与をすべてサポートし、それらのバリエーションを考慮します。
* 90 日間、30 分間など、指定したライフタイム値を持つアクセストークンをサポートします。
* 更新トークンの有無に関わらず、OAuth 2 認証フローがサポートされます。

## 認証コードを持つ OAuth 2 {#authorization-code}

宛先が標準の OAuth 2.0 認証コードフローをサポートしている場合 ( [RFC 標準仕様](https://tools.ietf.org/html/rfc6749#section-4.1)など ) またはそのバリエーションについては、以下の必須フィールドとオプションフィールドを参照してください。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| 認証コード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>authorizationUrl</b></li><li><b>accessTokenUrl</b></li><li>refreshTokenUrl</li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

この認証方法を宛先に設定するには、次の行を設定に追加します ( [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md):

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
| `authType` | String | 「OAUTH2」を使用します。 |
| `grant` | 文字列 | &quot;OAUTH2_AUTHORIZATION_CODE&quot;を使用します。 |
| `accessTokenUrl` | 文字列 | ユーザー側の URL。トークンにアクセスし、必要に応じて更新トークンを発行します。 |
| `authorizationUrl` | 文字列 | ユーザーをリダイレクトしてアプリケーションにログインするための認証サーバーの URL。 |
| `refreshTokenUrl` | 文字列 | *オプション。* 側の URL。更新トークンを発行します。 多くの場合、 `refreshTokenUrl` は `accessTokenUrl`. |
| `clientId` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント秘密鍵。 |
| `scope` | 文字列のリスト | *オプション*。リソースに対してExperience Platformが実行できるアクセストークンの範囲を設定します。 例：&quot;read, write&quot; |

{style="table-layout:auto"}

## パスワード付き OAuth 2

OAuth 2 のパスワード付与については、 [RFC 標準仕様](https://tools.ietf.org/html/rfc6749#section-4.3))、Experience Platformには、ユーザー名とパスワードが必要です。 認証フローでは、Experience Platformはこれらの資格情報をアクセストークンと、オプションで更新トークンと交換します。
Adobeは、以下の標準入力を利用して、値を上書きできるので、宛先の設定を簡略化します。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| パスワード | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li><li><b>ユーザー名</b></li><li><b>パスワード</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

>[!NOTE]
>
> 次のパラメーターを追加する必要はありません： `username` および `password` を次の設定に含めます。 追加する `"grant": "OAUTH2_PASSWORD"` 宛先の設定では、ユーザーが宛先に対して認証をおこなう際に、Experience PlatformUI でのユーザー名とパスワードの入力を求めます。

この認証方法を宛先に設定するには、次の行を設定に追加します ( [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md):

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
| `authType` | String | 「OAUTH2」を使用します。 |
| `grant` | 文字列 | 「OAUTH2_PASSWORD」を使用します。 |
| `accessTokenUrl` | 文字列 | ユーザー側の URL。トークンにアクセスし、必要に応じて更新トークンを発行します。 |
| `clientId` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント秘密鍵。 |
| `scope` | 文字列のリスト | *オプション*。リソースに対してExperience Platformが実行できるアクセストークンの範囲を設定します。 例：&quot;read, write&quot; |

{style="table-layout:auto"}

## クライアント資格情報付き OAuth 2 許可

OAuth 2 クライアント資格情報を設定できます ( [RFC 標準仕様](https://tools.ietf.org/html/rfc6749#section-4.4)) 宛先で使用できます。以下に示す標準の入力および出力をサポートします。 値をカスタマイズできます。 詳しくは、 [OAuth 2 設定のカスタマイズ](#customize-configuration) 」を参照してください。

| OAuth 2 付与 | 入力 | 出力 |
|---------|----------|---------|
| クライアント資格情報 | <ul><li><b>clientId</b></li><li><b>clientSecret</b></li><li>対象範囲</li><li><b>accessTokenUrl</b></li></ul> | <ul><li><b>accessToken</b></li><li>expiresIn</li><li>refreshToken</li><li>tokenType</li></ul> |

{style="table-layout:auto"}

この認証方法を宛先に設定するには、次の行を設定に追加します ( [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md):

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
| `authType` | String | 「OAUTH2」を使用します。 |
| `grant` | 文字列 | 「OAUTH2_CLIENT_CREDENTIALS」を使用します。 |
| `accessTokenUrl` | 文字列 | アクセストークンとオプションの更新トークンを発行する認証サーバーの URL。 |
| `refreshTokenUrl` | 文字列 | *オプション。* 側の URL。更新トークンを発行します。 多くの場合、 `refreshTokenUrl` は `accessTokenUrl`. |
| `clientId` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント ID。 |
| `clientSecret` | 文字列 | システムがAdobe Experience Platformに割り当てるクライアント秘密鍵。 |
| `scope` | 文字列のリスト | *オプション*。リソースに対してExperience Platformが実行できるアクセストークンの範囲を設定します。 例：&quot;read, write&quot; |

{style="table-layout:auto"}

## OAuth 2 設定のカスタマイズ {#customize-configuration}

上記の節で説明した設定は、標準の OAuth 2 付与について説明します。 ただし、Adobeが設計したシステムは柔軟性を提供し、OAuth 2 付与のバリエーションに対してカスタムパラメーターを使用できます。 標準の OAuth 2 設定をカスタマイズするには、 `authenticationDataFields` パラメーターを設定します。

### 例 1:使用 `authenticationDataFields` 認証応答から得られる情報を取り込む {#example-1}

この例では、宛先プラットフォームに、一定時間が経過すると期限切れになる更新トークンがあります。 この場合、パートナーは `refreshTokenExpiration` 更新トークンの有効期限を `refresh_token_expires_in` フィールドに値を入力します。

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

### 例 2:使用 `authenticationDataFields` 特別な更新トークンを提供する {#example-2}

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

### 例 3:ユーザーは、宛先を設定する際に、クライアント ID とクライアント秘密鍵を入力します {#example-3}

この例では、の節で示すように、グローバルクライアント ID とクライアント秘密鍵を作成する代わりに、を使用します。 [システムの前提条件](#prerequisites)の場合、顧客はクライアント ID、クライアントの秘密鍵、アカウント ID（顧客が宛先へのログインに使用する ID）を入力する必要があります

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



次のパラメーターを `authenticationDataFields` OAuth 2 設定をカスタマイズするには：

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationDataFields.name` | 文字列 | カスタムフィールドの名前。 |
| `authenticationDataFields.title` | 文字列 | カスタムフィールドに指定できるタイトル。 |
| `authenticationDataFields.description` | 文字列 | 設定したカスタムデータフィールドの説明。 |
| `authenticationDataFields.type` | 文字列 | カスタムデータフィールドのタイプを定義します。 <br> 指定できる値： `string`, `boolean`, `integer` |
| `authenticationDataFields.isRequired` | ブール値 | 認証フローでカスタムデータフィールドが必要かどうかを指定します。 |
| `authenticationDataFields.format` | 文字列 | 次を選択した場合： `"format":"password"`,Adobeは認証データフィールドの値を暗号化します。 と一緒に使用する場合 `"fieldType": "CUSTOMER"`を使用した場合、ユーザーがフィールドに入力する際の UI の入力も非表示になります。 |
| `authenticationDataFields.fieldType` | 文字列 | 宛先を「 」Experience Platformで設定した場合に、入力元がパートナー（自分）かユーザーかを示します。 |
| `authenticationDataFields.value` | 文字列. ブール値. 整数 | カスタムデータフィールドの値。 この値は、次の中から選択したタイプと一致します： `authenticationDataFields.type`. |
| `authenticationDataFields.authenticationResponsePath` | 文字列 | 参照する API 応答パスのフィールドを示します。 |

{style="table-layout:auto"}

## アクセストークンの更新 {#access-token-refresh}

Adobeは、ユーザーがプラットフォームにログインし直す必要なく、期限切れのアクセストークンを更新するシステムを設計しました。 システムで新しいトークンを生成できるので、宛先へのアクティベーションがシームレスに顧客に対して続行されます。

アクセストークンの更新を設定するには、Adobeが更新トークンを使用して新しいアクセストークンを取得できるように、テンプレート化された HTTP リクエストを設定する必要が生じる場合があります。 アクセストークンの有効期限が切れると、Adobeは指定したパラメーターを追加し、指定されたテンプレートリクエストを受け取ります。 以下を使用： `accessTokenRequest` パラメーターを使用して、アクセストークンの更新メカニズムを設定します。


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

次のパラメーターを `accessTokenRequest` トークンの更新プロセスをカスタマイズするには：

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `accessTokenRequest.destinationServerType` | 文字列 | `URL_BASED`.を使用します。 |
| `accessTokenRequest.urlBasedDestination.url.templatingStrategy` | 文字列 | <ul><li>用途 `PEBBLE_V1` テンプレートを `accessTokenRequest.urlBasedDestination.url.value`.</li><li> 用途 `NONE` フィールドの値が `accessTokenRequest.urlBasedDestination.url.value` は定数です。 </li></li> |
| `accessTokenRequest.urlBasedDestination.url.value` | 文字列 | Experience Platformがアクセストークンをリクエストする URL。 |
| `accessTokenRequest.httpTemplate.requestBody.templatingStrategy` | 文字列 | <ul><li>用途 `PEBBLE_V1` テンプレートを `accessTokenRequest.httpTemplate.requestBody.value`.</li><li> 用途 `NONE` フィールドの値が `accessTokenRequest.httpTemplate.requestBody.value` は定数です。 </li></li> |
| `accessTokenRequest.httpTemplate.requestBody.value` | 文字列 | テンプレート言語を使用して、アクセストークンエンドポイントへの HTTP リクエストのフィールドをカスタマイズします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、 [テンプレート規則](#templating-conventions) 」セクションに入力します。 |
| `accessTokenRequest.httpTemplate.httpMethod` | 文字列 | アクセストークンエンドポイントの呼び出しに使用する HTTP メソッドを指定します。 ほとんどの場合、この値は `POST`. |
| `accessTokenRequest.httpTemplate.contentType` | 文字列 | アクセストークンエンドポイントへの HTTP 呼び出しのコンテンツタイプを指定します。 <br> 例： `application/x-www-form-urlencoded` または `application/json`. |
| `accessTokenRequest.httpTemplate.headers` | 文字列 | アクセストークンエンドポイントへの HTTP 呼び出しにヘッダーを追加する必要があるかどうかを指定します。 |
| `accessTokenRequest.responseFields.templatingStrategy` | 文字列 | <ul><li>用途 `PEBBLE_V1` テンプレートを `accessTokenRequest.responseFields.value`.</li><li> 用途 `NONE` フィールドの値が `accessTokenRequest.responseFields.value` は定数です。 </li></li> |
| `accessTokenRequest.responseFields.value` | 文字列 | テンプレート言語を使用して、アクセストークンエンドポイントから HTTP 応答のフィールドにアクセスします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、 [テンプレート規則](#templating-conventions) 」セクションに入力します。 |
| `accessTokenRequest.validations.name` | 文字列 | この検証に指定した名前を示します。 |
| `accessTokenRequest.validations.actualValue.templatingStrategy` | 文字列 | <ul><li>用途 `PEBBLE_V1` テンプレートを `accessTokenRequest.validations.actualValue.value`.</li><li> 用途 `NONE` フィールドの値が `accessTokenRequest.validations.actualValue.value` は定数です。 </li></li> |
| `accessTokenRequest.validations.actualValue.value` | 文字列 | テンプレート言語を使用して、HTTP 応答のフィールドにアクセスします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、 [テンプレート規則](#templating-conventions) 」セクションに入力します。 |
| `accessTokenRequest.validations.expectedValue.templatingStrategy` | 文字列 | <ul><li>用途 `PEBBLE_V1` テンプレートを `accessTokenRequest.validations.expectedValue.value`.</li><li> 用途 `NONE` フィールドの値が `accessTokenRequest.validations.expectedValue.value` は定数です。 </li></li> |
| `accessTokenRequest.validations.expectedValue.value` | 文字列 | テンプレート言語を使用して、HTTP 応答のフィールドにアクセスします。 テンプレートを使用してフィールドをカスタマイズする方法について詳しくは、 [テンプレート規則](#templating-conventions) 」セクションに入力します。 |

{style="table-layout:auto"}

## テンプレート規則 {#templating-conventions}

認証のカスタマイズに応じて、前の節で示したように、認証応答のデータフィールドにアクセスする必要が生じる場合があります。 そのためには、 [Pebble テンプレート言語](https://pebbletemplates.io/) Adobeで使用され、以下のテンプレート規則を参照して OAuth 2 実装をカスタマイズします。


| プレフィックス | 説明 | 例 |
|---------|----------|---------|
| authData | パートナーまたは顧客データフィールドの値にアクセスする。 | ``{{ authData.accessToken }}`` |
| response.body | HTTP 応答本文 | ``{{ response.body.access_token }}`` |
| response.status | HTTP レスポンスステータス | ``{{ response.status }}`` |
| response.headers | HTTP 応答ヘッダー | ``{{ response.headers.server[0] }}`` |
| userContext | 現在の認証試行に関するアクセス情報 | <ul><li>`{{ userContext.sandboxName }} `</li><li>`{{ userContext.sandboxId }} `</li><li>`{{ userContext.imsOrgId }} `</li><li>`{{ userContext.client }} // the client executing the authentication attempt `</li></ul> |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むと、Adobe Experience Platformでサポートされる OAuth 2 認証パターンと、OAuth 2 認証サポートを使用して宛先を設定する方法を理解できます。 次に、「Destination SDK」を使用して、OAuth 2 がサポートされる宛先を設定できます。 読み取り [Destination SDKを使用した宛先の設定](../../guides/configure-destination-instructions.md) を参照してください。
