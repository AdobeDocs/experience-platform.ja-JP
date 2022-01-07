---
description: このページでは、「/authoring/destination-servers」 API エンドポイントを使用して実行できるすべての API 操作について説明します。 宛先のサーバーおよびテンプレートの仕様は、共通のエンドポイント「/authoring/destination-servers」を介してAdobe Experience Platform Destination SDKで設定できます。
title: 宛先サーバーエンドポイント API の操作
exl-id: a144b0fb-d34f-42d1-912b-8576296e59d2
source-git-commit: 6dd8a94e46b9bee6d1407e7ec945a722d8d7ecdb
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 7%

---

# 宛先サーバーエンドポイント API の操作

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destination-servers`

このページでは、 `/authoring/destination-servers` API エンドポイント。 宛先のサーバーおよびテンプレート仕様は、共通のエンドポイントを介してAdobe Experience Platform Destination SDKで設定できます `/authoring/destination-servers`. このエンドポイントが提供する機能については、 [サーバとテンプレートの仕様](./server-and-template-configuration.md).

## 宛先サーバー API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## 宛先サーバーの設定を作成する {#create}

新しい宛先サーバー設定を作成するには、 `/authoring/destination-servers` endpoint.

**API 形式**


```http
POST /authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい宛先サーバー設定を作成します。 以下のペイロードには、 `/authoring/destination-servers` endpoint. API 要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートをカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.region}}/items"
      }
   },
   "httpTemplate":{
      "httpMethod":"POST",
      "requestBody":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{ \"attributes\": [ {% for ns in [\"external_id\", \"yourdestination_id\"] %} {% if input.profile.identityMap[ns] is not empty and first_namespace_encountered %} , {% endif %} {% set first_namespace_encountered = true %} {% for identity in input.profile.identityMap[ns]%} { \"{{ ns }}\": \"{{ identity.id }}\" {% if input.profile.segmentMembership.ups is not empty %} , \"AEPSegments\": { \"add\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"realized\" or segment.value.status == \"existing\" %} {% if added_segment_found %} , {% endif %} {% set added_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ], \"remove\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"exited\" %} {% if removed_segment_found %} , {% endif %} {% set removed_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ] } {% set removed_segment_found = false %} {% set added_segment_found = false %} {% endif %} {% if input.profile.attributes is not empty %} , {% endif %} {% for attribute in input.profile.attributes %} \"{{ attribute.key }}\": {% if attribute.value is empty %} null {% else %} \"{{ attribute.value.value }}\" {% endif %} {% if not loop.last%} , {% endif %} {% endfor %} } {% if not loop.last %} , {% endif %} {% endfor %} {% endfor %} ] }"
      },
      "contentType":"application/json"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `name` | 文字列 | *必須* サーバーのわかりやすい名前を表し、Adobeのみに表示されます。 この名前は、パートナーや顧客には表示されません。 例 `Moviestar destination server`. |
| `destinationServerType` | 文字列 | *必須* `URL_BASED` は現在唯一の利用可能なオプションです。 |
| `urlBasedDestination.url.templatingStrategy` | 文字列 | *必須.* <ul><li>用途 `PEBBLE_V1` Adobeが `value` フィールドを参照してください。 次のようなエンドポイントがある場合は、このオプションを使用します。 `https://api.moviestar.com/data/{{customerData.region}}/items`. </li><li> 用途 `NONE` Adobe側で変換が必要ない場合（例えば、次のようなエンドポイントがある場合）、 `https://api.moviestar.com/data/items`.</li></ul> |
| `urlBasedDestination.url.value` | 文字列 | *必須* Experience Platformが接続する API エンドポイントのアドレスを入力します。 |
| `httpTemplate.httpMethod` | 文字列 | *必須* サーバーへの呼び出しでAdobeが使用するメソッド。 オプションは次のとおりです。 `GET`, `PUT`, `POST`, `DELETE`, `PATCH`. |
| `httpTemplate.requestBody.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `httpTemplate.requestBody.value` | 文字列 | *必須* この文字列は、Platform の顧客のデータを、サービスが想定する形式に変換する文字エスケープバージョンです。 <br> <ul><li> テンプレートの書き込み方法について詳しくは、 [テンプレートセクションの使用](./message-format.md#using-templating). </li><li> 文字のエスケープについて詳しくは、 [RFC JSON 標準、セクション 7](https://tools.ietf.org/html/rfc8259#section-7). </li><li> 単純な変換の例については、 [プロファイル属性](./message-format.md#attributes) 変換。 </li></ul> |
| `httpTemplate.contentType` | 文字列 | *必須* サーバーが受け入れるコンテンツタイプ。 この値は最も可能性が高い `application/json`. |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成された宛先サーバー設定の詳細を返します。

## 宛先サーバー設定のリスト {#retrieve-list}

IMS 組織のすべての宛先サーバー設定のリストを取得するには、にGETリクエストをおこないます `/authoring/destination-servers` endpoint.

**API 形式**


```http
GET /authoring/destination-servers
```

**リクエスト**

次のリクエストは、IMS 組織とサンドボックス設定に基づいて、アクセス権のある宛先サーバー設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある宛先サーバー設定のリストを返します。 1 `instanceId` は、1 つの宛先サーバーのテンプレートに対応します。 簡潔にするために、応答は切り捨てられます。

```json
{
   "items":[
      {
         "instanceId":"2307ec2b-4798-45a4-9239-5d0a2fb0ed67",
         "createdDate":"2020-11-17T06:49:24.331012Z",
         "lastModifiedDate":"2020-11-17T06:49:24.331012Z",
         "name":"Moviestar Destination Server",
         "destinationServerType":"URL_BASED",
         "urlBasedDestination":{
            "url":{
               "templatingStrategy":"PEBBLE_V1",
               "value":"https://go.{% if destination.config.domain == \"US\" %}moviestar.com{% else %}moviestar.eu{% endif%}/api/named_users/tags"
            }
         },
         "httpTemplate":{
            "requestBody":{
               "templatingStrategy":"PEBBLE_V1",
               "value":"{ \"audience\": { \"named_user_id\": [ {% for named_user in input.profile.identityMap.named_user_id %} \"{{ named_user.id }}\"{% if not loop.last %},{% endif %} {% endfor %} ] }, {% if addedSegments(input.profile.segmentMembership.ups) is not empty %} \"add\": { \"adobe-segments\": [ {% for added_segment in addedSegments(input.profile.segmentMembership.ups) %} \"{{ destination.segmentNames[added_segment.key] }}\"{% if not loop.last %},{% endif %} {% endfor %} ] } {% endif %} {% if addedSegments(input.profile.segmentMembership.ups) is not empty and removedSegments(input.profile.segmentMembership.ups) is not empty %} , {% endif %} {% if removedSegments(input.profile.segmentMembership.ups) is not empty %} \"remove\": { \"adobe-segments\": [ {% for removed_segment in removedSegments(input.profile.segmentMembership.ups) %} \"{{ destination.segmentNames[removed_segment.key] }}\"{% if not loop.last %},{% endif %} {% endfor %} ] } {% endif %} }"
            },
            "httpMethod":"POST",
            "contentType":"application/json",
            "headers":[
               {
                  "header":"Accept",
                  "value":{
                     "templatingStrategy":"NONE",
                     "value":"application/vnd.moviestar+json; version=3;"
                  }
               }
            ]
         },
         "qos":{
            "name":"freeform"
         }
      },
      {
         "instanceId":"d88de647-a352-4824-8b46-354afc7acbff",
         "createdDate":"2020-11-17T16:50:59.635228Z",
         "lastModifiedDate":"2020-11-17T16:50:59.635228Z",
         "name":"Test11 Destination Server",
         "destinationServerType":"URL_BASED",
         "urlBasedDestination":{
            "url":{
               "templatingStrategy":"PEBBLE_V1",
               "value":"https://go.{% if destination.config.domain == \"US\" %}moviestar.com{% else %}moviestar.eu{% endif%}/api/named_users/tags"
            }
         },
         "httpTemplate":{
            "requestBody":{
               "templatingStrategy":"PEBBLE_V1",
               "value":"{ \"audience\": { \"named_user_id\": [ {% for named_user in input.profile.identityMap.named_user_id %} \"{{ named_user.id }}\"{% if not loop.last %},{% endif %} {% endfor %} ] }, {% if addedSegments(input.profile.segmentMembership.ups) is not empty %} \"add\": { \"adobe-segments\": [ {% for added_segment in addedSegments(input.profile.segmentMembership.ups) %} \"{{ destination.segmentNames[added_segment.key] }}\"{% if not loop.last %},{% endif %} {% endfor %} ] } {% endif %} {% if addedSegments(input.profile.segmentMembership.ups) is not empty and removedSegments(input.profile.segmentMembership.ups) is not empty %} , {% endif %} {% if removedSegments(input.profile.segmentMembership.ups) is not empty %} \"remove\": { \"adobe-segments\": [ {% for removed_segment in removedSegments(input.profile.segmentMembership.ups) %} \"{{ destination.segmentNames[removed_segment.key] }}\"{% if not loop.last %},{% endif %} {% endfor %} ] } {% endif %} }"
            },
            "httpMethod":"POST",
            "contentType":"application/json",
            "headers":[
               {
                  "header":"Accept",
                  "value":{
                     "templatingStrategy":"NONE",
                     "value":"application/vnd.moviestar+json; version=3;"
                  }
               }
            ]
         },
         "qos":{
            "name":"freeform"
         }
      }
   ]
}
    
```

## 既存の宛先サーバー設定の更新 {#update}

既存の宛先サーバー設定を更新するには、 `/authoring/destination-servers` エンドポイントを作成し、更新する宛先サーバー設定のインスタンス ID を指定します。 呼び出しの本文で、更新された宛先サーバーの設定を指定します。

**API 形式**


```http
PUT /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先サーバー設定の ID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された、既存の宛先サーバー設定を更新します。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/destination-servers/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.region}}/items"
      }
   },
   "httpTemplate":{
      "httpMethod":"POST",
      "requestBody":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{ \"attributes\": [ {% for ns in [\"external_id\", \"yourdestination_id\"] %} {% if input.profile.identityMap[ns] is not empty and first_namespace_encountered %} , {% endif %} {% set first_namespace_encountered = true %} {% for identity in input.profile.identityMap[ns]%} { \"{{ ns }}\": \"{{ identity.id }}\" {% if input.profile.segmentMembership.ups is not empty %} , \"AEPSegments\": { \"add\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"realized\" or segment.value.status == \"existing\" %} {% if added_segment_found %} , {% endif %} {% set added_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ], \"remove\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"exited\" %} {% if removed_segment_found %} , {% endif %} {% set removed_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ] } {% set removed_segment_found = false %} {% set added_segment_found = false %} {% endif %} {% if input.profile.attributes is not empty %} , {% endif %} {% for attribute in input.profile.attributes %} \"{{ attribute.key }}\": {% if attribute.value is empty %} null {% else %} \"{{ attribute.value.value }}\" {% endif %} {% if not loop.last%} , {% endif %} {% endfor %} } {% if not loop.last %} , {% endif %} {% endfor %} {% endfor %} ] }"
      },
      "contentType":"application/json"
   }
}
```





## 特定の宛先サーバー設定の取得 {#get}

特定の宛先サーバー設定に関する詳細な情報を取得するには、 `/authoring/destination-servers` エンドポイントを作成し、更新する宛先サーバー設定のインスタンス ID を指定します。

**API 形式**


```http
GET /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する宛先サーバー設定の ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destination-servers/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された宛先サーバー設定に関する詳細情報を返します。

```json
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.region}}/items"
      }
   },
   "httpTemplate":{
      "httpMethod":"POST",
      "requestBody":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{ \"attributes\": [ {% for ns in [\"external_id\", \"yourdestination_id\"] %} {% if input.profile.identityMap[ns] is not empty and first_namespace_encountered %} , {% endif %} {% set first_namespace_encountered = true %} {% for identity in input.profile.identityMap[ns]%} { \"{{ ns }}\": \"{{ identity.id }}\" {% if input.profile.segmentMembership.ups is not empty %} , \"AEPSegments\": { \"add\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"realized\" or segment.value.status == \"existing\" %} {% if added_segment_found %} , {% endif %} {% set added_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ], \"remove\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"exited\" %} {% if removed_segment_found %} , {% endif %} {% set removed_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ] } {% set removed_segment_found = false %} {% set added_segment_found = false %} {% endif %} {% if input.profile.attributes is not empty %} , {% endif %} {% for attribute in input.profile.attributes %} \"{{ attribute.key }}\": {% if attribute.value is empty %} null {% else %} \"{{ attribute.value.value }}\" {% endif %} {% if not loop.last%} , {% endif %} {% endfor %} } {% if not loop.last %} , {% endif %} {% endfor %} {% endfor %} ] }"
      },
      "contentType":"application/json"
   }
}
```


## 特定の宛先サーバー設定の削除 {#delete}

指定した宛先サーバー設定を削除するには、 `/authoring/destination-servers` エンドポイントを探し、リクエストパスで削除する宛先サーバー設定の ID を指定します。

**API 形式**

```http
DELETE /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | この `id` 削除する宛先サーバー設定の情報です。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/destination-servers/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と空の HTTP 応答を返します。

## API エラー処理

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順

このドキュメントを読んだ後、 `/authoring/destination-servers` API エンドポイント。 読み取り [宛先の設定にDestination SDKを使用する方法](./configure-destination-instructions.md) を参照して、この手順が宛先を設定するプロセスに適した場所を把握します。
