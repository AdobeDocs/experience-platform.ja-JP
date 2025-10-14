---
description: オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新または削除します。アドビは、マーケティング API の仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。テンプレートを定義、テスト、送信すると、アドビはこのテンプレートを使用して、宛先への API 呼び出しを構造化します。
title: オーディエンスメタデータの管理
exl-id: 795e8adb-c595-4ac5-8d1a-7940608d01cd
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1309'
ht-degree: 73%

---

# オーディエンスメタデータの管理

オーディエンスメタデータテンプレートを使用して、宛先のオーディエンスをプログラムで作成、更新または削除します。アドビは、マーケティング API の仕様に基づいて設定できる、拡張可能なオーディエンスメタデータテンプレートを提供します。設定を定義、テスト、送信すると、アドビはこの設定を使用して、宛先への API 呼び出しを構造化します。

`/authoring/audience-templates` API エンドポイントを使用して、このドキュメントで説明する機能を設定できます。エンドポイントで実行できる操作の完全なリストには、[メタデータテンプレートの作成](../metadata-api/create-audience-template.md)を参照してください。

## オーディエンスメタデータ管理エンドポイントを使用するタイミング {#when-to-use}

API の設定に応じて、Experience Platform で宛先を設定する際に、オーディエンスメタデータ管理エンドポイントを使用する必要がある場合と必要がない場合があります。以下の決定ツリー図を使用して、オーディエンスメタデータエンドポイントを使用するタイミングと、宛先に対するオーディエンスメタデータテンプレートの設定方法を理解します。

![決定ツリー図](../assets/functionality/audience-metadata-decision-tree.png)

## オーディエンスメタデータ管理でサポートされるユースケース {#use-cases}

Destination SDKでのオーディエンスメタデータのサポートを使用すると、Experience Platformの宛先を設定する際に、Experience Platform ユーザーがオーディエンスをマッピングして宛先に対してアクティブ化する際に、複数あるオプションの 1 つを指定できます。 宛先設定の[オーディエンスメタデータ設定](../functionality/destination-configuration/audience-metadata-configuration.md)セクションのパラメーターで、ユーザーが利用できるオプションを制御できます。

### ユースケース 1 - サードパーティの API を使用しており、ユーザーはマッピング ID を入力する必要がない場合

オーディエンスを作成、更新または削除する API エンドポイントがある場合は、オーディエンスメタデータテンプレートを使用して、オーディエンスの作成、更新または削除エンドポイントの仕様に合わせて Destination SDK を設定できます。Experience Platform は、オーディエンスをプログラムで作成、更新または削除して、メタデータを Experience Platform に同期することができます。

Experience Platform ユーザーインターフェイス（UI）で宛先に対してオーディエンスをアクティブ化する際に、アクティブ化ワークフローのオーディエンスマッピング ID フィールドに手動で入力する必要はありません。

### ユースケース 2 - ユーザーはまず宛先でオーディエンスを作成する必要があり、マッピング ID を手動で入力する必要がある場合

オーディエンスや他のメタデータをパートナーやユーザーが手動で宛先に作成する必要がある場合、ユーザーは、アクティブ化ワークフローのオーディエンスマッピング ID フィールドに手動で入力して、宛先と Experience Platform の間でオーディエンスメタデータを同期させる必要があります。

![入力マッピング ID](../assets/functionality/input-mapping-id.png)

### ユースケース 3 - 宛先で Experience Platform オーディエンス ID を使用でき、ユーザーがマッピング ID を手動で入力する必要がない場合

宛先のシステムで Experience Platform オーディエンス ID を使用できる場合は、オーディエンスメタデータテンプレートでそれを設定できます。ユーザーは、セグメントをアクティブ化する際に、オーディエンスマッピング ID を入力する必要はありません。

## 汎用で拡張可能なオーディエンステンプレート {#generic-and-extensible}

前述の使用例をサポートするために、アドビでは、API の仕様に合わせてカスタマイズできる汎用テンプレートを提供しています。

お使いの API が以下をサポートしている場合、汎用テンプレートを使用して[新しいオーディエンステンプレートを作成](../metadata-api/create-audience-template.md)できます。

* HTTP メソッド：POST、GET、PUT、DELETE、PATCH。
* 認証タイプは、OAuth 1、OAuth 2（更新トークン）、OAuth 2（Bearer トークン）です。
* 関数は、オーディエンスの作成、オーディエンスの更新、オーディエンスの取得、オーディエンスの削除、資格情報の検証を行うことができます。

アドビのエンジニアリングチームが協力して、カスタムフィールドを含む汎用テンプレートを拡張できます（使用例が必要な場合）。


## サポートされるテンプレートイベント {#supported-events}

次の表に、オーディエンスメタデータテンプレートでサポートされるイベントを示します。

| テンプレートセクション | 説明 |
|--- |--- |
| `create` | API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォームのセグメントやオーディエンスをプログラムで作成し、情報を Adobe Experience Platform に同期します。 |
| `update` | API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォームのセグメントやオーディエンスをプログラムで更新し、情報を Adobe Experience Platform に同期します。 |
| `delete` | API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォームのセグメントやオーディエンスをプログラムで削除します。 |
| `validate` | パートナー API を呼び出す前に、テンプレート設定のすべてのフィールドの検証を実行します。例えば、ユーザーのアカウント ID が正しく入力されていることを検証できます。 |
| `notify` | ファイルベースの宛先にのみ適用されます。API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、ファイルの書き出しが正常に行われたことを通知します。 |
| `createDestination` | API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォームのデータフローをプログラムで作成し、情報をAdobe Experience Platformに同期します。 |
| `updateDestination` | API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォームのデータフローをプログラムで更新し、情報をAdobe Experience Platformに同期します。 |
| `deleteDestination` | API への HTTP 呼び出しに必要なすべてのコンポーネント（URL、HTTP メソッド、ヘッダー、リクエストおよび応答本文）を含めて、プラットフォームからデータフローをプログラムで削除します。 |

{style="table-layout:auto"}

## 設定例 {#configuration-examples}

この節では、参照用に、汎用オーディエンスメタデータ設定の例を示します。

URL、ヘッダー、リクエスト本文は、3 つの設定例でどのように異なるかに注意してください。 これは、3 つのサンプルプラットフォームのマーケティング API の仕様が異なるためです。

一部の例では、`{{authData.accessToken}}` または `{{segment.name}}` などのマクロフィールドは URL で、その他の例ではヘッダーまたはリクエスト本文で使用されます。使用方法は、マーケティング API の仕様に応じて異なります。

+++ストリーミングの例 1

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

+++

+++ストリーミングの例 2

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

+++

+++ストリーミングの例 3

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

+++

+++ファイルベースの例

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
      "notify":{
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

+++

テンプレートのすべてのパラメーターについては、[オーディエンステンプレートの作成](../metadata-api/create-audience-template.md) API リファレンスを参照してください。

## オーディエンスメタデータテンプレートで使用されるマクロ {#macros}

Experience Platform と API の間でオーディエンス ID、アクセストークン、エラーメッセージなどの情報を渡すために、オーディエンステンプレートには使用できるマクロが含まれています。このページの 3 つの設定例で使用されるマクロの説明を以下に示します。

| マクロ | 説明 |
|--- |--- |
| `{{segment.alias}}` | Experience Platform でオーディエンスエイリアスにアクセスできるようにします。 |
| `{{segment.name}}` | Experience Platform でオーディエンス名にアクセスできるようにします。 |
| `{{segment.id}}` | Experience Platform でオーディエンス ID にアクセスできるようにします。 |
| `{{customerData.accountId}}` | 宛先設定で設定したアカウント ID フィールドにアクセスできるようにします。 |
| `{{oauth2ServiceAccessToken}}` | OAuth 2 の設定に基づいて、アクセストークンを動的に生成できるようにします。 |
| `{{authData.accessToken}}` | API エンドポイントにアクセストークンを渡すことができるようにします。Experience Platform が期限切れでないトークンを使用して宛先に接続する場合は、`{{authData.accessToken}}` を使用します。そうでない場合は、`{{oauth2ServiceAccessToken}}` を使用してアクセストークンを生成します。 |
| `{{body.segments[0].segment.id}}` | 作成されたオーディエンスの一意の ID をキー `externalAudienceId` の値として返します。 |
| `{{error.message}}` | Experience Platform UI でユーザーに表示されるエラーメッセージを返します。 |
| `{{{segmentEnrichmentAttributes}}}` | 特定のオーディエンスのすべてのエンリッチメント属性にアクセスできます。  このマクロは、`create`、`update`、および `delete` イベントでサポートされています。 エンリッチメント属性は、[&#x200B; カスタムアップロードオーディエンス &#x200B;](destination-configuration/schema-configuration.md#external-audiences) に対してのみ使用できます。 エンリッチメント属性の選択の仕組みについては、[&#x200B; バッチオーディエンスアクティベーションガイド &#x200B;](../../ui/activate-batch-profile-destinations.md#select-enrichment-attributes) を参照してください。 |
| `{{destination.name}}` | 宛先の名前を返します。 |
| `{{destination.sandboxName}}` | 宛先が設定されているExperience Platform サンドボックスの名前を返します。 |
| `{{destination.id}}` | 宛先設定の ID を返します。 |
| `{{destination.imsOrgId}}` | 宛先が設定されている IMS 組織 ID を返します。 |
| `{{destination.enrichmentAttributes}}` | 宛先にマッピングされたすべてのオーディエンスのすべてのエンリッチメント属性にアクセスできるようにします。 このマクロは、`createDestination`、`updateDestination`、および `deleteDestination` イベントでサポートされています。 エンリッチメント属性は、[&#x200B; カスタムアップロードオーディエンス &#x200B;](destination-configuration/schema-configuration.md#external-audiences) に対してのみ使用できます。 エンリッチメント属性の選択の仕組みについては、[&#x200B; バッチオーディエンスアクティベーションガイド &#x200B;](../../ui/activate-batch-profile-destinations.md#select-enrichment-attributes) を参照してください。 |
| `{{destination.enrichmentAttributes.<namespace>.<segmentId>}}` | 宛先にマッピングされた特定の外部オーディエンスのエンリッチメント属性にアクセスできます。 エンリッチメント属性は、[&#x200B; カスタムアップロードオーディエンス &#x200B;](destination-configuration/schema-configuration.md#external-audiences) に対してのみ使用できます。 エンリッチメント属性の選択の仕組みについては、[&#x200B; バッチオーディエンスアクティベーションガイド &#x200B;](../../ui/activate-batch-profile-destinations.md#select-enrichment-attributes) を参照してください。 |

{style="table-layout:auto"}
