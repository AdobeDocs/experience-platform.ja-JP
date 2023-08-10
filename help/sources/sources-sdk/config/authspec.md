---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: セルフサービスソース（バッチ SDK）の認証仕様の設定
description: このドキュメントでは、セルフサービスソース（バッチ SDK）を使用するために準備する必要がある設定の概要を説明します。
exl-id: 68ed22fe-1f22-46d2-9d58-72ad8a9e6b98
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 4%

---

# セルフサービスソース（バッチ SDK）の認証仕様の設定

認証仕様は、Adobe Experience Platformユーザーがソースに接続する方法を定義します。

The `authSpec` 配列には、ソースを Platform に接続するために必要な認証パラメーターに関する情報が含まれています。 任意のソースが複数の異なる種類の認証をサポートできます。

## 認証仕様

セルフサービスソース（バッチ SDK）では、OAuth 2 更新コードと基本認証をサポートしています。 OAuth 2 更新コードと基本認証の使用に関するガイダンスについては、以下の表を参照してください

### OAuth 2 更新コード

OAuth 2 の更新コードは、一時的なアクセストークンと更新トークンを生成することで、アプリケーションへの安全なアクセスを可能にします。 アクセストークンを使用すると、他の資格情報を提供することなく、リソースに安全にアクセスできます。更新トークンを使用すると、アクセストークンの期限が切れた後に新しいアクセストークンを生成できます。

```json
{
  "name": "OAuth2 Refresh Code",
  "type": "OAuth2RefreshCode",
  "spec": {
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "description": "Define auth params required for connecting to generic rest using oauth2 authorization code.",
    "properties": {
      "authorizationTestUrl": {
        "description": "Authorization test url to validate accessToken.",
        "type": "string"
      },
      "clientId": {
        "description": "Client id of user account.",
        "type": "string"
      },
      "clientSecret": {
        "description": "Client secret of user account.",
        "type": "string",
        "format": "password"
      },
      "accessToken": {
        "description": "Access Token",
        "type": "string",
        "format": "password"
      },
      "refreshToken": {
        "description": "Refresh Token",
        "type": "string",
        "format": "password"
      },
      "expirationDate": {
        "description": "Date of token expiry.",
        "type": "string",
        "format": "date",
        "uiAttributes": {
          "hidden": true
        }
      },
      "accessTokenUrl": {
        "description": "Access token url to fetch access token.",
        "type": "string"
      },
      "requestParameterOverride": {
        "type": "object",
        "description": "Specify parameter to override.",
        "properties": {
          "accessTokenField": {
            "description": "Access token field name to override.",
            "type": "string"
          },
          "refreshTokenField": {
            "description": "Refresh token field name to override.",
            "type": "string"
          },
          "expireInField": {
            "description": "ExpireIn field name to override.",
            "type": "string"
          },
          "authenticationMethod": {
            "description": "Authentication method override.",
            "type": "string",
            "enum": [
              "GET",
              "POST"
            ]
          },
          "clientId": {
            "description": "ClientId field name override.",
            "type": "string"
          },
          "clientSecret": {
            "description": "ClientSecret field name override.",
            "type": "string"
          }
        }
      }
    },
    "required": [
      "accessToken"
    ]
  }
}
```

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `authSpec.name` | サポートされている認証タイプの名前を表示します。 | `oAuth2-refresh-code` |
| `authSpec.type` | ソースでサポートされる認証のタイプを定義します。 | `oAuth2-refresh-code` |
| `authSpec.spec` | 認証のスキーマ、データタイプ、プロパティに関する情報が含まれます。 |
| `authSpec.spec.$schema` | 認証に使用するスキーマを定義します。 | `http://json-schema.org/draft-07/schema#` |
| `authSpec.spec.type` | スキーマのデータ型を定義します。 | `object` |
| `authSpec.spec.properties` | 認証に使用される資格情報に関する情報が含まれます。 |
| `authSpec.spec.properties.description` | 秘密鍵証明書に関する簡単な説明を表示します。 |
| `authSpec.spec.properties.type` | 秘密鍵証明書のデータ型を定義します。 | `string` |
| `authSpec.spec.properties.clientId` | アプリケーションに関連付けられたクライアント ID。 クライアント ID は、アクセストークンを取得するために、クライアントの秘密鍵と組み合わせて使用されます。 |
| `authSpec.spec.properties.clientSecret` | アプリケーションに関連付けられたクライアント秘密鍵。 クライアント秘密鍵は、クライアント ID と組み合わせて使用し、アクセストークンを取得します。 |
| `authSpec.spec.properties.accessToken` | アクセストークンは、アプリケーションへのセキュアなアクセスを許可します。 |
| `authSpec.spec.properties.refreshToken` | 更新トークンは、アクセストークンの有効期限が切れる際に、新しいアクセストークンの生成に使用されます。 |
| `authSpec.spec.properties.expirationDate` | アクセストークンの有効期限を定義します。 |
| `authSpec.spec.properties.refreshTokenUrl` | 更新トークンを取得するために使用する URL。 |
| `authSpec.spec.properties.accessTokenUrl` | 更新トークンを取得するために使用する URL。 |
| `authSpec.spec.properties.requestParameterOverride` | 認証時に上書きする秘密鍵証明書のパラメーターを指定できます。 |
| `authSpec.spec.required` | 認証に必要な資格情報が表示されます。 | `accessToken` |

{style="table-layout:auto"}


### 基本認証

基本認証は、アカウントのユーザー名とアカウントのパスワードを組み合わせて使用し、アプリケーションにアクセスできる認証タイプです。

```json
{
  "name": "Basic Authentication",
  "type": "BasicAuthentication",
  "spec": {
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "description": "defines auth params required for connecting to rest service.",
    "properties": {
      "username": {
        "description": "Username to connect rest endpoint.",
        "type": "string"
      },
      "password": {
        "description": "Password to connect rest endpoint.",
        "type": "string",
        "format": "password"
      }
    },
    "required": [
      "username",
      "password"
    ]
  }
}
```

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `authSpec.name` | サポートされている認証タイプの名前を表示します。 | `Basic Authentication` |
| `authSpec.type` | ソースでサポートされる認証のタイプを定義します。 | `BasicAuthentication` |
| `authSpec.spec` | 認証のスキーマ、データタイプ、プロパティに関する情報が含まれます。 |
| `authSpec.spec.$schema` | 認証に使用するスキーマを定義します。 | `http://json-schema.org/draft-07/schema#` |
| `authSpec.spec.type` | スキーマのデータ型を定義します。 | `object` |
| `authSpec.spec.description` | 認証タイプに特有の詳細情報が表示されます。 |
| `authSpec.spec.properties` | 認証に使用される資格情報に関する情報が含まれます。 |
| `authSpec.spec.properties.username` | アプリケーションに関連付けられたアカウントのユーザー名。 |
| `authSpec.spec.properties.password` | アプリケーションに関連付けられたアカウントのパスワード。 |
| `authSpec.spec.required` | Platform で入力する必須の値として必要なフィールドを指定します。 | `username` |

{style="table-layout:auto"}

## 認証仕様の例

次に、 [[!DNL MailChimp Members]](../../tutorials/api/create/marketing-automation/mailchimp-members.md) ソース。

```json
  "authSpec": [
    {
      "name": "OAuth2 Refresh Code",
      "type": "OAuth2RefreshCode",
      "spec": {
        "$schema": "http://json-schema.org/draft-07/schema#",
        "type": "object",
        "description": "Define auth params required for connecting to generic rest using oauth2 authorization code.",
        "properties": {
          "authorizationTestUrl": {
            "description": "Authorization test url to validate accessToken.",
            "type": "string"
          },
          "accessToken": {
            "description": "Access Token of mailChimp endpoint.",
            "type": "string",
            "format": "password"
          }
        },
        "required": [
          "accessToken"
        ]
      }
    },
    {
      "name": "Basic Authentication",
      "type": "BasicAuthentication",
      "spec": {
        "$schema": "http://json-schema.org/draft-07/schema#",
        "type": "object",
        "description": "defines auth params required for connecting to rest service.",
        "properties": {
          "username": {
            "description": "Username to connect mailChimp endpoint.",
            "type": "string"
          },
          "password": {
            "description": "Password to connect mailChimp endpoint.",
            "type": "string",
            "format": "password"
          }
        },
        "required": [
          "username",
          "password"
        ]
      }
    }
  ],
```

## 次の手順

認証仕様を入力したら、Platform に統合するソースのソース仕様を設定できます。 次のドキュメントを参照してください： [ソース仕様の構成](./sourcespec.md) を参照してください。
