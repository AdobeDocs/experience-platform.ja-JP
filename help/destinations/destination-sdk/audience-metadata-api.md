---
description: このページでは、「/authoring/audience-templates」 APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。
title: オーディエンスメタデータエンドポイントAPIの操作
source-git-commit: 19307fba8f722babe5b6d57e80735ffde00fc851
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 5%

---

# オーディエンスメタデータエンドポイントAPIの操作

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/audience-templates`

このページでは、`/authoring/audience-templates` APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。 このエンドポイントを使用するタイミングについては、[オーディエンスメタデータ管理](./audience-metadata-management.md)をお読みください。

## オーディエンステンプレートAPI操作の概要 {#get-started}

続行する前に、[はじめに](./getting-started.md)を参照し、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 新しいオーディエンステンプレートの作成 {#create}

`/authoring/audience-templates`エンドポイントにPOSTリクエストを送信して、新しいオーディエンステンプレートを作成できます。

**API 形式**


```http
POST /authoring/audience-templates
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターによって設定された、新しいオーディエンスメタデータテンプレートを作成します。 以下のペイロードには、`/authoring/audience-templates`エンドポイントで受け入れられるすべてのパラメーターが含まれます。 APIの要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートがカスタマイズ可能であることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/audience-templates \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `name` | 文字列 | 宛先のオーディエンスメタデータテンプレートの名前。 この名前は、Experience Platformユーザーインターフェイス内のパートナー固有のエラーメッセージに表示され、その後`metadataTemplate.create.errorSchemaMap`から解析されたエラーメッセージが続きます。 |
| `url` | 文字列 | APIのURLとエンドポイント。プラットフォームでオーディエンス/セグメントを作成、更新、削除または検証するために使用されます。 次の2つの業界の例があります。`https://adsapi.snapchat.com/v1/adaccounts/{{customerData.accountId}}/segments`と`https://api.linkedin.com/v2/dmpSegments/{{segment.alias}}`が表示されます。 |
| `httpMethod` | 文字列 | 宛先のセグメント/オーディエンスをプログラムで作成、更新、削除または検証するために、エンドポイントで使用されるメソッド。 例：`POST`、`PUT`、`DELETE` |
| `headers.header` | 文字列 | APIの呼び出しに追加する必要があるHTTPヘッダーを指定します。 例：`"Content-Type"`。 |
| `headers.value` | 文字列 | APIへの呼び出しに追加するHTTPヘッダーの値を指定します。 例：`"application/x-www-form-urlencoded"`。 |
| `requestBody` | 文字列 | APIに送信するメッセージ本文のコンテンツを指定します。 `requestBody`オブジェクトに追加する必要のあるパラメーターは、APIが受け入れるフィールドによって異なります。 例については、オーディエンスメタデータ機能のドキュメントの[最初のテンプレートの例](./audience-metadata-management.md#example-1)を参照してください。 |
| `responseFields.name` | 文字列 | APIが呼び出されたときに返される応答フィールドを指定します。 例については、オーディエンスメタデータ機能のドキュメントの[テンプレートの例](./audience-metadata-management.md#examples)を参照してください。 |
| `responseFields.value` | 文字列 | APIが呼び出されたときに返される応答フィールドの値を指定します。 |
| `responseErrorFields.name` | 文字列 | APIが呼び出されたときに返される応答フィールドを指定します。 例については、オーディエンスメタデータ機能のドキュメントの[テンプレートの例](./audience-metadata-management.md#examples)を参照してください。 |
| `responseErrorFields.value` | 文字列 | 宛先からのAPI呼び出し応答で返されたエラーメッセージを解析します。 これらのエラーメッセージは、ユーザーユーザーインターフェイスでExperience Platformに表示されます。 |
| `validations.field` | 文字列 | API呼び出しが宛先に対しておこなわれる前に、検証をフィールドに対して実行する必要があるかどうかを示します。 例えば、`{{validations.accountId}}`を使用して、ユーザーのアカウントIDを検証できます。 |
| `validations.regex` | 文字列 | 検証が合格するためのフィールドの構造を示します。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTPステータス200と、新しく作成されたオーディエンステンプレートの詳細を返します。

## オーディエンステンプレートの更新 {#update}

`/authoring/audience-templates`エンドポイントにPUTリクエストを送信し、更新するオーディエンステンプレートのインスタンスIDを指定することで、既存のオーディエンステンプレートを更新できます。 呼び出しの本文に、更新されたテンプレートを指定します。

**API 形式**


```http
PUT /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新するオーディエンスメタデータテンプレートのID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された、既存のオーディエンスメタデータテンプレートを更新します。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/audience-templates/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

`/authoring/audience-templates`エンドポイントに対してGETリクエストを実行することで、IMS組織のすべてのオーディエンステンプレートのリストを取得できます。

**API 形式**


```http
GET /authoring/audience-templates
```

**リクエスト**

次のリクエストは、IMS組織とサンドボックス設定に基づいて、アクセス権のあるオーディエンステンプレートのリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/audience-templates \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用したIMS組織IDとサンドボックス名に基づいて、アクセス権のあるオーディエンスメタデータテンプレートのリストと共にHTTPステータス200を返します。 1つの`instanceId`は、1つの宛先のテンプレートに対応します。 簡潔にするために応答は切り捨てられます。

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

`/authoring/audience-templates`エンドポイントにGETリクエストを送信し、取得するオーディエンステンプレートのインスタンスIDを指定することで、特定のオーディエンステンプレートに関する詳細な情報を取得できます。

**API 形式**


```http
GET /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得するオーディエンスメタデータテンプレートのID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/audience-templates/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTPステータス200と、指定されたオーディエンステンプレートに関する詳細情報を返します。

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

`/authoring/audience-templates`エンドポイントにDELETEリクエストを送信し、リクエストパスに削除するオーディエンステンプレートのIDを指定することで、指定したオーディエンステンプレートを削除できます。

**API 形式**

```http
DELETE /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除するオーディエンステンプレートの`id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/audience-templates/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
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

このドキュメントを読むと、オーディエンスメタデータテンプレートを使用するタイミングと、 `/authoring/audience-templates` APIエンドポイントを使用してオーディエンスメタデータテンプレートを設定する方法がわかります。 [宛先SDKを使用して宛先](./configure-destination-instructions.md)を設定する方法を読み、この手順が宛先の設定プロセスにどのように適合するかを理解してください。
