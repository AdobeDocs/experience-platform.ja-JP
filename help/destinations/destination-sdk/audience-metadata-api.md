---
description: このページでは、API エンドポイント /authoring/audience-templates を使用して実行できるすべての API 操作について説明します。
title: オーディエンスメタデータエンドポイント API の操作
exl-id: 3444da8c-b2be-4254-980a-8cce7560134d
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 91%

---

# オーディエンスメタデータエンドポイント API の操作

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/audience-templates`

このページでは、`/authoring/audience-templates` API エンドポイントを使用して実行できるすべての API の操作について説明します。このエンドポイントを使用するタイミングについては、[オーディエンスメタデータ管理](./audience-metadata-management.md)を参照してください。

## オーディエンステンプレート API 操作の概要 {#get-started}

続ける前に「[はじめる前に](./getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 新しいオーディエンステンプレートの作成 {#create}

エンドポイント `/authoring/audience-templates` に POST リクエストを実行することで、新しいオーディエンステンプレートを作成できます。

**API 形式**

```http
POST /authoring/audience-templates
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しいオーディエンスメタデータテンプレートを作成します。以下のペイロードには、エンドポイント `/authoring/audience-templates` が受け取れるパラメーターがすべて含まれます。呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートは API 要件に応じてカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/audience-templates \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "metadataTemplate":{
      "name":"string",
      "create":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "update":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "delete":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "validate":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "notify":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      }
   },
   "validations":[
      {
         "field":"string",
         "regex":"string"
      }
   ]
}'
```

| プロパティ | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `name` | 文字列 | 宛先のオーディエンスメタデータテンプレートの名前。 この名前は、Adobe Experience Platform のユーザーインターフェイスであらゆるパートナー固有のエラーメッセージに表示され、その後に `metadataTemplate.create.errorSchemaMap` から解析されたエラーメッセージが続きます。 |
| `url` | 文字列 | API の URL とエンドポイント。プラットフォームでオーディエンスやセグメントを作成、更新、削除、検証するために使用します。 `https://adsapi.snapchat.com/v1/adaccounts/{{customerData.accountId}}/segments` および `https://api.linkedin.com/v2/dmpSegments/{{segment.alias}}` は 2 つの業界の例です。 |
| `httpMethod` | 文字列 | 宛先のセグメントやオーディエンスをプログラムで作成、更新、削除、検証するためにエンドポイントで使用されるメソッド。例：`POST`、`PUT`、`DELETE` |
| `headers.header` | 文字列 | API への呼び出しに追加する HTTP ヘッダーを指定します。例：`"Content-Type"` |
| `headers.value` | 文字列 | API への呼び出しに追加する HTTP ヘッダーの値を指定します。 例：`"application/x-www-form-urlencoded"` |
| `requestBody` | 文字列 | API に送信するメッセージ本文のコンテンツを指定します。 `requestBody` オブジェクトに追加する必要があるパラメーターは、API が受け入れるフィールドに応じて異なります。例については、オーディエンスメタデータ機能ドキュメントの[最初のテンプレートの例](./audience-metadata-management.md#example-1)を参照してください。 |
| `responseFields.name` | 文字列 | 呼び出し時に API が返す応答フィールドを指定します。例については、オーディエンスメタデータ機能ドキュメントの[テンプレートの例](./audience-metadata-management.md#examples)を参照してください。 |
| `responseFields.value` | 文字列 | 呼び出し時に API が返す応答フィールドの値を指定します。 |
| `responseErrorFields.name` | 文字列 | 呼び出し時に API が返す応答フィールドを指定します。例については、オーディエンスメタデータ機能ドキュメントの[テンプレートの例](./audience-metadata-management.md#examples)を参照してください。 |
| `responseErrorFields.value` | 文字列 | API 呼び出しに対する宛先からの応答で返されたエラーメッセージを解析します。これらのエラーメッセージは、Adobe Experience Platform のユーザーインターフェイスでユーザーに表示されます。 |
| `validations.field` | 文字列 | 宛先に対する API 呼び出しが実行される前に、いずれかのフィールドに対して検証を実行する必要があるかどうかを示します。例えば、ユーザーのアカウント ID の検証に `{{validations.accountId}}` を使用できます。 |
| `validations.regex` | 文字列 | 検証に合格するために、フィールドをどのように構成するべきかを示します。 |

{style="table-layout:auto"}

**応答**

リクエストが成功した場合は、HTTP ステータス 200と新しく作成されたオーディエンステンプレートの詳細が返されます。

## オーディエンステンプレートの更新 {#update}

エンドポイント `/authoring/audience-templates` に PUT リクエストを実行し、更新するオーディエンステンプレートのインスタンス ID を指定することで、既存のオーディエンステンプレートを更新できます。呼び出しの本文で、更新したテンプレートを指定します。

**API 形式**

```http
PUT /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新するオーディエンスメタデータテンプレートの ID。 |

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、既存のオーディエンスメタデータテンプレートを更新します。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/audience-templates/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
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

## オーディエンステンプレートのリストの取得 {#retrieve-list}

組織のすべてのオーディエンステンプレートのリストを取得するには、 `/authoring/audience-templates` endpoint.

**API 形式**


```http
GET /authoring/audience-templates
```

**リクエスト**

次のリクエストは、組織とサンドボックスの設定に基づいて、アクセス権のあるオーディエンステンプレートのリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/audience-templates \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用した組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のあるオーディエンスメタデータテンプレートのリストを返します。 1 つの `instanceId` が 1 つの宛先のテンプレートに対応します。簡潔にするために、応答は切り捨てられます。

```json
{
   "items":[
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
   ]
}
```

## 特定のオーディエンステンプレートの取得 {#get}

エンドポイント `/authoring/audience-templates` に GET リクエストを実行し、取得するオーディエンステンプレートのインスタンス ID を指定することで、特定のオーディエンステンプレートに関する詳細な情報を取得できます。

**API 形式**

```http
GET /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得するオーディエンスメタデータテンプレートの ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/audience-templates/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、指定されたオーディエンステンプレートの詳細情報とともに HTTP ステータス 200 が返されます。

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


## 特定のオーディエンステンプレートの削除 {#delete}

エンドポイント `/authoring/audience-templates` に DELETE リクエストを実行し、リクエストパスで削除するオーディエンステンプレートの ID を指定することで、指定したオーディエンステンプレートを削除できます。

**API 形式**

```http
DELETE /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除するオーディエンステンプレートの `id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/audience-templates/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、オーディエンスメタデータテンプレートを使用するタイミングと、`/authoring/audience-templates` API エンドポイントを使用してオーディエンスメタデータテンプレートを設定する方法について確認しました。[Destination SDK を使用して宛先を設定する方法](./configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。
