---
description: オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新、削除します。 Adobeは、マーケティング API の仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。 テンプレートを定義、テスト、送信すると、Adobeはこのテンプレートを使用して、宛先への API 呼び出しを構造化します。
title: Audience metadata management
exl-id: 795e8adb-c595-4ac5-8d1a-7940608d01cd
source-git-commit: cb4e399798a9521e6f3da89cbd88d19476ab070d
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 1%

---

# Audience metadata management {#audience-metadata-management}

## 概要 {#overview}

オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新、削除します。 Adobeは、マーケティング API の仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。 設定を定義、テスト、送信すると、Adobeはこの設定を使用して、宛先への API 呼び出しを構造化します。

このドキュメントで説明する機能は、 `/authoring/audience-templates` API エンドポイント。 読み取り [オーディエンスメタデータエンドポイント API の操作](./audience-metadata-api.md) エンドポイントで実行できる操作の完全なリストについては、を参照してください。

## Audience Metadata Management エンドポイントを使用するタイミング {#when-to-use}

API 設定に応じて、Experience Platformで宛先を設定する際に、Audience Metadata Management エンドポイントを使用する必要がある場合とならない場合があります。 以下のデシジョンツリー図を使用して、オーディエンスメタデータエンドポイントを使用するタイミングと、宛先に対するオーディエンスメタデータテンプレートの設定方法を理解します。

![デシジョンツリー図](./assets/audience-metadata-decision-tree.png)

## Audience Metadata Management でサポートされる使用例 {#use-cases}

Destination SDKでのオーディエンスメタデータのサポートを使用すると、Experience Platformの宛先を設定する際に、Platform ユーザーがセグメントをマッピングし、宛先に対してアクティブ化する際に、いくつかのオプションの 1 つを指定できます。 ユーザーが使用できるオプションは、 [宛先設定](./destination-configuration.md#segment-mapping).

### 使用例 1 — サードパーティの API があり、ユーザーはマッピング ID を入力する必要がない

セグメントやオーディエンスを作成、更新、削除する API エンドポイントがある場合は、オーディエンスメタデータテンプレートを使用して、セグメントの作成、更新、削除エンドポイントの仕様に合わせてDestination SDKを設定できます。 Experience Platformは、セグメントをプログラムで作成/更新/削除し、メタデータをExperience Platformに同期できます。

Experience Platformユーザーインターフェイス (UI) で宛先に対してセグメントをアクティブ化する際に、アクティベーションワークフローのセグメントマッピング ID フィールドに手動で入力する必要はありません。

### 使用例 2 — ユーザーは最初に宛先でセグメントを作成する必要があり、マッピング ID を手動で入力する必要がある

セグメントや他のメタデータをパートナーやユーザーが手動で宛先に作成する必要がある場合は、Experience Platformがアクティベーションワークフローの「セグメントマッピング ID 」フィールドに手動で入力し、宛先と宛先の間でセグメントメタデータを同期します。

![入力マッピング ID](./assets/input-mapping-id.png)

### 使用例 3 — 宛先がExperience Platformセグメント ID を受け入れる、ユーザーがマッピング ID を手動で入力する必要がない

宛先システムがExperience Platformセグメント ID を受け入れる場合、オーディエンスメタデータテンプレートで設定できます。 ユーザーは、セグメントをアクティブ化する際に、セグメントマッピング ID を入力する必要はありません。

## 汎用で拡張可能なオーディエンステンプレート {#generic-and-extensible}

上記の使用例をサポートするために、Adobeでは、API の仕様に合わせてカスタマイズできる汎用テンプレートを提供しています。

汎用テンプレートを使用して、 [新しいオーディエンステンプレートの作成](./audience-metadata-api.md#create) お使いの API が以下をサポートしている場合：

* HTTP メソッド：POST,GET,PUT,DELETE,PATCH
* 認証タイプは次のとおりです。OAuth 1、OAuth 2（更新トークン）、OAuth 2（bearer トークン）
* 関数は次のとおりです。オーディエンスの作成、オーディエンスの更新、オーディエンスの取得、オーディエンスの削除、資格情報の検証

Adobeエンジニアリングチームが協力して、カスタムフィールドを含む汎用テンプレートを拡張できます（使用例が必要な場合）。

## 設定例 {#configuration-examples}

この節では、参照用の 3 つの汎用オーディエンスメタデータ設定の例と、設定の主な節の説明を示します。 URL、ヘッダー、リクエスト、応答本文は、3 つの設定例でどのように異なるかに注意してください。 これは、3 つのサンプルプラットフォームのマーケティング API の仕様が異なるためです。

一部の例では、 `{{authData.accessToken}}` または `{{segment.name}}` は URL で使用され、その他の例ではヘッダーまたはリクエスト本文で使用されます。 実際の動作はマーケティング API の仕様によって異なります。

| テンプレートセクション | 説明 |
|--- |--- |
| `create` | API への HTTP 呼び出しをおこなうために必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォーム内のセグメント/オーディエンスをプログラムで作成し、情報をAdobe Experience Platformに同期します。 |
| `update` | API への HTTP 呼び出しをおこなうために必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含め、プラットフォームのセグメント/オーディエンスをプログラムで更新し、情報をAdobe Experience Platformに同期します。 |
| `delete` | API に対する HTTP 呼び出しをおこない、プラットフォーム内のセグメント/オーディエンスをプログラムで削除するために必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含みます。 |
| `validate` | パートナー API を呼び出す前に、テンプレート設定のすべてのフィールドの検証を実行します。 例えば、ユーザーのアカウント ID が正しく入力されていることを検証できます。 |

{style=&quot;table-layout:auto&quot;}

### 最初の例 {#example-1}

```json
{
   "instanceId":"34ab9cc2-2536-44a5-9dc5-b2fea60b3bd6",
   "createdDate":"2021-07-26T19:30:52.012490Z",
   "lastModifiedDate":"2021-07-27T21:25:42.763478Z",
   "metadataTemplate":{
      "create":{
         "url":"https://adsapi.moviestar.com/v1/adaccounts/{{customerData.accountId}}/segments",
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
         "url":"https://adsapi.moviestar.com/v1/adaccounts/{{customerData.accountId}}/segments/{{segment.alias}}",
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
         "url":"https://adsapi.moviestar.com/v1/adaccounts/{{customerData.accountId}}/segments/{{segment.alias}}",
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

テンプレート内のすべてのパラメーターの説明については、リファレンスドキュメントを参照してください。 [オーディエンスメタデータエンドポイント API の操作](./audience-metadata-api.md).

## オーディエンスメタデータテンプレートで使用されるマクロ

Experience Platformと API の間でセグメント ID、アクセストークン、エラーメッセージなどの情報を渡すために、オーディエンステンプレートには使用できるマクロが含まれています。 このページの 3 つの設定例で使用されるマクロの説明を以下に示します。

| マクロ | 説明 |
|--- |--- |
| `{{segment.alias}}` | 「 」でセグメントエイリアスにアクセスできるExperience Platform。 |
| `{{segment.name}}` | 「 」でセグメント名にアクセスできます。Experience Platform |
| `{{segment.id}}` | 「 」でセグメント ID にアクセスできるExperience Platform。 |
| `{{customerData.accountId}}` | 宛先設定で設定したアカウント ID フィールドにアクセスできます。 |
| `{{oauth2ServiceAccessToken}}` | OAuth 2 の設定に基づいて、アクセストークンを動的に生成できます。 |
| `{{authData.accessToken}}` | API エンドポイントにアクセストークンを渡すことができます。 用途 `{{authData.accessToken}}` Experience Platformが期限切れでないトークンを使用して宛先に接続する場合は、 `{{oauth2ServiceAccessToken}}` をクリックしてアクセストークンを生成します。 |
| `{{body.segments[0].segment.id}}` | 作成されたオーディエンスの一意の識別子をキーの値として返します `externalAudienceId`. |
| `{{error.message}}` | ユーザー UI でユーザーに表示されるエラーメッセージをExperience Platformに返します。 |

{style=&quot;table-layout:auto&quot;}
