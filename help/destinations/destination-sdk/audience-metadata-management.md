---
description: オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新、削除します。 Adobeは、マーケティングAPIの仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。 テンプレートを定義、テスト、送信すると、Adobeがそのテンプレートを使用して宛先へのAPI呼び出しを構造化します。
title: Audience Metadata Management
source-git-commit: d2452bf0e59866d3deca57090001c4c5a0935525
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 1%

---

# Audience Metadata Management {#audience-metadata-management}

## 概要 {#overview}

オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新、削除します。 Adobeは、マーケティングAPIの仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。 設定を定義、テスト、送信すると、Adobeはこの設定を使用して宛先へのAPI呼び出しを構造化します。

このドキュメントで説明する機能は、`/authoring/audience-templates` APIエンドポイントを使用して設定できます。 エンドポイントで実行できる操作の完全なリストについては、[オーディエンスメタデータエンドポイントAPIの操作](./audience-metadata-api.md)をお読みください。

## オーディエンスメタデータ管理エンドポイントを使用するタイミング {#when-to-use}

APIの設定によっては、Experience Platformで宛先を設定する際に、Audience Metadata Managementエンドポイントを使用する必要がある場合とならない場合があります。 以下のデシジョンツリー図を使用して、オーディエンスメタデータエンドポイントを使用するタイミングと、宛先に対するオーディエンスメタデータテンプレートの設定方法を理解します。

![デシジョンツリー図](./assets/audience-metadata-decision-tree.png)

## オーディエンスメタデータ管理でサポートされる使用例 {#use-cases}

宛先SDKでのオーディエンスメタデータのサポートを使用すると、Experience Platformの宛先を設定する際に、Platformユーザーが宛先にセグメントをマッピングし、アクティブ化する際に、いくつかのオプションの1つを指定できます。 [宛先設定](./destination-configuration.md#segment-mapping)のセグメントマッピングセクションのパラメーターを使用して、ユーザーが使用できるオプションを制御できます。

### 使用例1 — サードパーティのAPIがあり、ユーザーはマッピングIDを入力する必要がない

セグメントまたはオーディエンスを作成/更新/削除するAPIエンドポイントがある場合は、オーディエンスメタデータテンプレートを使用して、セグメントの作成/更新/削除エンドポイントの仕様に合わせて宛先SDKを設定できます。 Experience Platformは、プログラムによってセグメントを作成/更新/削除し、メタデータをExperience Platformに同期できます。

Experience Platformユーザーインターフェイス(UI)で宛先に対してセグメントをアクティブ化する際に、アクティブ化ワークフローのセグメントマッピングIDフィールドに手動で入力する必要はありません。

### 使用例2 — ユーザーは最初に宛先にセグメントを作成する必要があり、マッピングIDを手動で入力する必要がある

セグメントやその他のメタデータをパートナーまたはユーザーが宛先に手動で作成する必要がある場合は、アクティベーションワークフローの「セグメントマッピングID 」フィールドに手動で入力して、宛先とExperience Platformの間でセグメントメタデータを同期する必要があります。

![Input mapping ID](./assets/input-mapping-id.png)

### 使用例3 — 宛先がExperience PlatformセグメントIDを受け入れる場合、ユーザーはマッピングIDを手動で入力する必要はありません

宛先システムがExperience PlatformセグメントIDを受け入れる場合は、オーディエンスメタデータテンプレートでこれを設定できます。 ユーザーは、セグメントをアクティブ化する際に、セグメントマッピングIDを入力する必要はありません。

## 汎用の拡張可能なオーディエンステンプレート {#generic-and-extensible}

上記の使用例をサポートするために、Adobeでは、API仕様に合わせてカスタマイズ可能な汎用テンプレートを提供しています。

APIが以下をサポートしている場合は、汎用テンプレートを使用して[新しいオーディエンステンプレート](./audience-metadata-api.md#create)を作成できます。

* HTTPメソッド：POST,GET,PUT,DELETE,PATCH
* 認証タイプは次のとおりです。OAuth 1、OAuth 2（更新トークンあり）、OAuth 2（bearerトークンあり）
* 関数は次のとおりです。オーディエンスの作成、オーディエンスの更新、オーディエンスの取得、オーディエンスの削除、資格情報の検証

Adobeエンジニアリングチームは、ユースケースで必要な場合は、ユーザーと協力して、カスタムフィールドを含む汎用テンプレートを拡張できます。

## テンプレートの例 {#template-examples}

この節では、参照用の3つの一般的なオーディエンスメタデータ設定の例と、設定の主なセクションの説明を示します。 URL、ヘッダー、リクエスト、応答本文の違いに注意してください。 これは、3つのサンプルプラットフォームのマーケティングAPIの仕様が異なるためです。

一部の例では、`{{authData.accessToken}}`や`{{segment.name}}`などのマクロフィールドがURLで使用され、他の例では、ヘッダーやリクエスト本文で使用されます。 実際には、マーケティングAPIの仕様によって異なります。

| テンプレートセクション | 説明 |
|--- |--- |
| `create` | APIへのHTTP呼び出しをおこなうために必要なすべてのコンポーネント（URL、HTTPメソッド、ヘッダー、リクエストおよび応答本文）を含め、プラットフォームでセグメント/オーディエンスをプログラムで作成し、情報をAdobe Experience Platformに同期します。 |
| `update` | APIへのHTTP呼び出しをおこなうために必要なすべてのコンポーネント（URL、HTTPメソッド、ヘッダー、リクエストおよび応答本文）を含め、プラットフォームのセグメント/オーディエンスをプログラムで更新し、情報をAdobe Experience Platformに同期します。 |
| `delete` | APIへのHTTP呼び出しをおこない、プラットフォーム内のセグメント/オーディエンスをプログラムで削除するために必要なすべてのコンポーネント（URL、HTTPメソッド、ヘッダー、リクエスト、応答本文）が含まれます。 |
| `validations` | パートナーAPIを呼び出す前に、テンプレート設定内のすべてのフィールドの検証を実行します。 例えば、ユーザーのアカウントIDが正しく入力されていることを検証できます。 |

{style=&quot;table-layout:auto&quot;}

### 最初の例 {#example-1}

```json
{
   "instanceId":"34ab9cc2-2536-44a5-9dc5-b2fea60b3bd6",
   "createdDate":"2021-07-26T19:30:52.012490Z",
   "lastModifiedDate":"2021-07-27T21:25:42.763478Z",
   "metadataTemplate":{
      "create":{
         "url":"https://api.moviestar.com/v1/adaccounts/{{customerData.accountId}}/segments",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/json",
               "header":"Content-Type"
            },
            {
               "value":"Bearer {{oauth2ServiceAccessToken}}",
               "header":"Authorization"
            }
         ],
         "requestBody":{
            "json":{
               "segments":[
                  {
                     "name":"{{segment.name}}",
                     "description":"{{segment.description}}",
                     "source_type":"FIRST_PARTY",
                     "ad_account_id":"{{customerData.accountId}}",
                     "retention_in_days":180
                  }
               ]
            }
         },
         "responseFields":[
            {
               "value":"{{body.segments[0].segment.id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{root}}",
               "name":"message"
            }
         ]
      },
      "update":{
         "url":"https://adsapi.moviestar.com/v1/adaccounts/{{customerData.accountId}}/segments",
         "httpMethod":"PUT",
         "headers":[
            {
               "value":"application/json",
               "header":"Content-Type"
            },
            {
               "value":"Bearer {{oauth2ServiceAccessToken}}",
               "header":"Authorization"
            }
         ],
         "requestBody":{
            "json":{
               "segments":[
                  {
                     "id":"{{segment.alias}}",
                     "name":"{{segment.name}}",
                     "description":"{{segment.description}}"
                  }
               ]
            }
         },
         "responseFields":[
            {
               "value":"{{body.segments[0].segment.id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{root}}",
               "name":"message"
            }
         ]
      },
      "delete":{
         "url":"https://adsapi.moviestar.com/v1/segments/{{segment.alias}}",
         "httpMethod":"DELETE",
         "headers":[
            {
               "value":"application/json",
               "header":"Content-Type"
            },
            {
               "value":"Bearer {{oauth2ServiceAccessToken}}",
               "header":"Authorization"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{root}}",
               "name":"message"
            }
         ]
      },
      "name":"Moviestar destination audience template - Example 1"
   }
}
```

### 2 つ目の例： {#example-2}

```json
{
   "instanceId":"12c78017-5af3-4d4e-8f9c-d330c547c482",
   "createdDate":"2021-07-20T13:27:37.029490Z",
   "lastModifiedDate":"2021-07-20T18:53:03.622306Z",
   "metadataTemplate":{
      "create":{
         "url":"https://api.moviestar.com/v1.0/{{customerData.accountId}}/customaudiences?fields=name,description,account_id&subtype=CUSTOM&name={{segment.name}}&customer_file_source={{segment.metadata.customer_file_source}}&access_token={{authData.accessToken}}",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseFields":[
            {
               "value":"{{response.id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      },
      "update":{
         "url":"https://api.moviestar.com/v1.0/{{segment.alias}}?field=name,description,account_id&access_token={{authData.accessToken}}&customerAudienceId={{segment.alias}}&&name={{segment.name}}&description={{segment.description}}&customer_file_source={{segment.metadata.customer_file_source}}",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseFields":[
            {
               "value":"{{response.id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      },
      "delete":{
         "url":"https://api.moviestar.com/v1.0/{{segment.alias}}?fields=name,description,account_id&access_token={{authData.accessToken}}&customerAudienceId={{segment.alias}}",
         "httpMethod":"DELETE",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      },
      "validate":{
         "url":"https://api.moviestar.com/v1.0/permissions?access_token={{authData.accessToken}}",
         "httpMethod":"GET",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseFields":[
            {
               "value":"{{response.data[0].permission}}",
               "name":"Id"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      }
   }
}
```

### 3番目の例 {#example-3}

```json
{
   "instanceId":"12a3238f-b509-4a40-b8fb-0a5006e7901d",
   "createdDate":"2021-07-20T13:30:30.843054Z",
   "lastModifiedDate":"2021-07-21T16:33:05.787472Z",
   "metadataTemplate":{
      "create":{
         "url":"https://api.moviestar.com/v2/dmpSegments",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/json",
               "header":"Content-Type"
            },
            {
               "value":"Bearer {{authData.accessToken}}",
               "header":"Authorization"
            }
         ],
         "requestBody":{
            "json":{
               "name":"{{segment.name}}",
               "type":"USER",
               "account":"{{customerData.accountId}}",
               "accessPolicy":"PRIVATE",
               "destinations":[
                  {
                     "destination":"MOVIESTAR"
                  }
               ],
               "sourcePlatform":"ADOBE"
            }
         },
         "responseFields":[
            {
               "value":"{{headers.x-moviestar-id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{message}}",
               "name":"message"
            }
         ]
      },
      "update":{
         "url":"https://api.moviestar.com/v2/dmpSegments/{{segment.alias}}",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/json",
               "header":"Content-Type"
            },
            {
               "value":"Bearer {{authData.accessToken}}",
               "header":"Authorization"
            }
         ],
         "requestBody":{
            "json":{
               "patch":{
                  "$set":{
                     "name":"{{segment.name}}"
                  }
               }
            }
         },
         "responseErrorFields":[
            {
               "value":"{{message}}",
               "name":"message"
            }
         ]
      },
      "delete":{
         "url":"https://api.moviestar.com/v2/dmpSegments/{{segment.alias}}",
         "httpMethod":"DELETE",
         "headers":[
            {
               "value":"application/json",
               "header":"Content-Type"
            },
            {
               "value":"Bearer {{authData.accessToken}}",
               "header":"Authorization"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{message}}",
               "name":"message"
            }
         ]
      },
      "name":"Moviestar audience template - Third example"
   }
}
```

参照ドキュメント[オーディエンスメタデータエンドポイントAPIの操作](./audience-metadata-api.md)で、テンプレート内のすべてのパラメーターの説明を確認できます。

## オーディエンスメタデータテンプレートで使用されるマクロ

Experience PlatformとAPIの間でセグメントID、アクセストークン、エラーメッセージなどの情報を渡すために、オーディエンステンプレートには使用できるマクロが含まれています。 このページの3つの設定例で使用されるマクロの説明を以下に示します。

| マクロ | 説明 |
|--- |--- |
| `{{segment.alias}}` | 「 」セグメントのエイリアスにExperience Platformでアクセスできます。 |
| `{{segment.name}}` | 「 」セグメント名にアクセスできます。Experience Platform |
| `{{segment.id}}` | セグメントIDにアクセスできるExperience Platform。 |
| `{{customerData.accountId}}` | 宛先設定で設定したアカウントIDフィールドにアクセスできます。 |
| `{{oauth2ServiceAccessToken}}` | OAuth 2の設定に基づいて、アクセストークンを動的に生成できます。 |
| `{{authData.accessToken}}` | アクセストークンをAPIエンドポイントに渡すことができます。 Experience Platformが期限切れでないトークンを使用して宛先に接続する場合は`{{authData.accessToken}}`を使用し、それ以外の場合は`{{oauth2ServiceAccessToken}}`を使用してアクセストークンを生成します。 |
| `{{body.segments[0].segment.id}}` | 作成されたオーディエンスの一意の識別子をキー`externalAudienceId`の値として返します。 |
| `{{error.message}}` | エラーUIでユーザーに表示されるエラーExperience Platformを返します。 |

{style=&quot;table-layout:auto&quot;}
