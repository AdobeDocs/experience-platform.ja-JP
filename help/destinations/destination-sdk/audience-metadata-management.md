---
description: オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新、削除します。 Adobeは、マーケティング API の仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。 テンプレートを定義、テスト、送信すると、Adobeはこのテンプレートを使用して、宛先への API 呼び出しを構造化します。
title: Audience metadata management
exl-id: 795e8adb-c595-4ac5-8d1a-7940608d01cd
source-git-commit: 397c49284c30c648695a7a186d3f3e76a2675807
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 1%

---

# Audience metadata management {#audience-metadata-management}

## 概要 {#overview}

オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新、削除します。 Adobeは、マーケティング API の仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。 設定を定義、テスト、送信すると、Adobeはこの設定を使用して、宛先への API 呼び出しを構造化します。

このドキュメントで説明する機能は、`/authoring/audience-templates` API エンドポイントを使用して設定できます。 エンドポイントで実行できる操作の完全なリストについては、[ オーディエンスメタデータエンドポイント API の操作 ](./audience-metadata-api.md) をお読みください。

## オーディエンスメタデータ管理エンドポイントを使用するタイミング {#when-to-use}

API 設定によっては、Audience で宛先を設定する際に、Audience Metadata Management エンドポイントを使用する必要がある場合とならない場合があります。Experience Platform 以下のデシジョンツリー図を使用して、オーディエンスメタデータエンドポイントを使用するタイミングと、宛先に対するオーディエンスメタデータテンプレートの設定方法を理解します。

![デシジョンツリー図](./assets/audience-metadata-decision-tree.png)

## Audience Metadata Management でサポートされる使用例 {#use-cases}

宛先 SDK でのオーディエンスメタデータのサポートを使用すると、Experience Platformの宛先を設定する際に、Platform ユーザーが宛先にセグメントをマッピングし、アクティブ化する際に、いくつかのオプションの 1 つを指定できます。 [ 宛先設定 ](./destination-configuration.md#segment-mapping) のセグメントマッピングセクションのパラメーターを使用して、ユーザーが使用できるオプションを制御できます。

### 使用例 1 — サードパーティの API を持っていて、ユーザーはマッピング ID を入力する必要がない

セグメントやオーディエンスを作成、更新、削除する API エンドポイントがある場合は、オーディエンスメタデータテンプレートを使用して、セグメントの作成、更新、削除エンドポイントの仕様に合わせて宛先 SDK を設定できます。 Experience Platformは、プログラムによってセグメントを作成、更新、削除し、メタデータをExperience Platformに同期できます。

Experience Platformユーザーインターフェイス (UI) で宛先に対してセグメントをアクティブ化する場合、アクティベーションワークフローのセグメントマッピング ID フィールドに手動で入力する必要はありません。

### 使用例 2 — ユーザーは最初に宛先にセグメントを作成する必要があり、マッピング ID を手動で入力する必要がある

宛先でパートナーやユーザーが手動でセグメントやその他のメタデータを作成する必要がある場合は、アクティベーションワークフローの「セグメントマッピング ID 」フィールドに手動で入力して、宛先とExperience Platformの間でセグメントメタデータを同期する必要があります。

![Input mapping ID](./assets/input-mapping-id.png)

### 使用例 3 — 宛先がExperience Platformセグメント ID を受け入れる、ユーザーがマッピング ID を手動で入力する必要がない

宛先システムがExperience Platformセグメント ID を受け入れる場合は、オーディエンスメタデータテンプレートで設定できます。 ユーザーは、セグメントをアクティブ化する際に、セグメントマッピング ID を設定する必要はありません。

## 汎用の拡張可能なオーディエンステンプレート {#generic-and-extensible}

上記の使用例をサポートするため、Adobeは、API の仕様に合わせてカスタマイズ可能な汎用テンプレートを提供します。

API が以下をサポートしている場合は、汎用テンプレートを使用して [ 新しいオーディエンステンプレート ](./audience-metadata-api.md#create) を作成できます。

* HTTP メソッド：POST,GET,PUT,DELETE,PATCH
* 認証タイプは次のとおりです。OAuth 1、OAuth 2（更新トークンあり）、OAuth 2（bearer トークンあり）
* 関数は次のとおりです。オーディエンスの作成、オーディエンスの更新、オーディエンスの取得、オーディエンスの削除、資格情報の検証

Adobeエンジニアリングチームは、ユースケースで必要な場合は、ユーザーと協力して、カスタムフィールドを含む汎用テンプレートを展開できます。

## 設定例 {#configuration-examples}

この節では、参照用の 3 つの一般的なオーディエンスメタデータ設定の例と、設定の主なセクションの説明を示します。 URL、ヘッダー、リクエスト、応答本文の違いに注意してください。 これは、3 つのサンプルプラットフォームのマーケティング API の仕様が異なるためです。

一部の例では、`{{authData.accessToken}}` や `{{segment.name}}` などのマクロフィールドが URL で使用され、他の例では、これらのフィールドがヘッダーやリクエスト本文で使用されます。 実際には、マーケティング API の仕様によって異なります。

| テンプレートセクション | 説明 |
|--- |--- |
| `create` | API への HTTP 呼び出しをおこなうために必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めます。これらのコンポーネントを使用して、プラットフォーム内にセグメント/オーディエンスをプログラムで作成し、情報をAdobe Experience Platformに同期します。 |
| `update` | API への HTTP 呼び出しをおこなうために必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めます。これにより、プラットフォームのセグメント/オーディエンスをプログラムで更新し、情報をAdobe Experience Platformに同期できます。 |
| `delete` | API への HTTP 呼び出しをおこない、プラットフォーム内のセグメント/オーディエンスをプログラムで削除するために必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含みます。 |
| `validations` | パートナー API を呼び出す前に、テンプレート設定内のすべてのフィールドの検証を実行します。 例えば、ユーザーのアカウント ID が正しく入力されていることを検証できます。 |

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

### 3 番目の例 {#example-3}

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

テンプレート内のすべてのパラメーターの説明については、リファレンスドキュメント [ オーディエンスメタデータエンドポイント API の操作 ](./audience-metadata-api.md) を参照してください。

## オーディエンスメタデータテンプレートで使用されるマクロ

Experience Platformと API の間でセグメント ID、アクセストークン、エラーメッセージなどの情報を渡すために、オーディエンステンプレートには使用できるマクロが含まれています。 このページの 3 つの設定例で使用されるマクロの説明を以下に示します。

| マクロ | 説明 |
|--- |--- |
| `{{segment.alias}}` | 「 」セグメントのエイリアスにExperience Platformでアクセスできます。 |
| `{{segment.name}}` | 「 」でセグメント名にアクセスできます。Experience Platform |
| `{{segment.id}}` | Experience Platformのセグメント ID にアクセスできます。 |
| `{{customerData.accountId}}` | 宛先設定で設定したアカウント ID フィールドにアクセスできます。 |
| `{{oauth2ServiceAccessToken}}` | OAuth 2 の設定に基づいて、アクセストークンを動的に生成できます。 |
| `{{authData.accessToken}}` | アクセストークンを API エンドポイントに渡すことができます。 Experience Platformが期限切れでないトークンを使用して宛先に接続する場合は `{{authData.accessToken}}` を使用し、期限切れでない場合は `{{oauth2ServiceAccessToken}}` を使用してアクセストークンを生成します。 |
| `{{body.segments[0].segment.id}}` | 作成したオーディエンスの一意の識別子をキー `externalAudienceId` の値として返します。 |
| `{{error.message}}` | エラー UI でユーザーに表示されるエラーExperience Platformを返します。 |

{style=&quot;table-layout:auto&quot;}
