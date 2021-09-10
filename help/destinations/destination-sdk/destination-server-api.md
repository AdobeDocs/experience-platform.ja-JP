---
description: このページでは、「/authoring/destination-servers」 APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。 宛先のサーバー仕様とテンプレート仕様は、共通のエンドポイント「/authoring/destination-servers」を使用して、Adobe Experience Platform Destination SDKで設定できます。
title: 宛先サーバーエンドポイントAPIの操作
exl-id: a144b0fb-d34f-42d1-912b-8576296e59d2
source-git-commit: bd65cfa557fb42d23022578b98bc5482e8bd50b1
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 5%

---

# 宛先サーバーエンドポイントAPIの操作

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destination-servers`

このページでは、`/authoring/destination-servers` APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。 宛先のサーバー仕様とテンプレート仕様は、共通のエンドポイント`/authoring/destination-servers`を介してAdobe Experience Platform Destination SDKで設定できます。 このエンドポイントで提供される機能の説明については、[サーバーとテンプレートの仕様](./server-and-template-configuration.md)を参照してください。

## 宛先サーバーAPI操作の概要 {#get-started}

続行する前に、[はじめに](./getting-started.md)を参照し、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 宛先サーバーの設定の作成 {#create}

`/authoring/destination-servers`エンドポイントにPOSTリクエストを送信して、新しい宛先サーバー設定を作成できます。

**API 形式**


```http
POST /authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい宛先サーバー設定を作成します。 以下のペイロードには、`/authoring/destination-servers`エンドポイントで受け入れられるすべてのパラメーターが含まれます。 APIの要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートがカスタマイズ可能であることに注意してください。

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
| `name` | 文字列 | サーバーのわかりやすい名前を表し、Adobeにのみ表示されます。 この名前は、パートナーや顧客には表示されません。 例 `Moviestar destination server`. |
| `destinationServerType` | 文字列 | `URL_BASED` は現在、唯一のオプションです。 |
| `urlBasedDestination.url.templatingStrategy` | 文字列 | <ul><li>Adobeが下の`value`フィールドのURLを変換する必要がある場合は、`PEBBLE_V1`を使用します。 次のようなエンドポイントがある場合は、このオプションを使用します。`https://api.moviestar.com/data/{{customerData.region}}/items`. </li><li> Adobe側に変換が必要ない場合は、`NONE`を使用します。例えば、次のようなエンドポイントがある場合などです。`https://api.moviestar.com/data/items`.</li></ul> |
| `urlBasedDestination.url.value` | 文字列 | Experience Platformが接続するAPIエンドポイントのアドレスを入力します。 |
| `urlBasedDestination.maxUsersPerRequest` | 整数 | Adobeは、1回のHTTP呼び出しで、書き出された複数のプロファイルを集計できます。 1回のHTTP呼び出しでエンドポイントが受け取るプロファイルの最大数を指定します。 これは、ベストエフォートの集計です。 例えば、値を100に指定した場合、Adobeは1回の呼び出しで100未満の任意の数のプロファイルを送信できます。 <br> サーバーがリクエストごとに複数のユーザーを受け入れない場合、この値を1に設定します。 |
| `urlBasedDestination.splitUserById` | Boolean | 宛先への呼び出しをIDで分割する必要がある場合は、このフラグを使用します。 サーバーが呼び出しごとに1つのIDしか受け入れない場合、このフラグを`true`に設定します。 |
| `httpTemplate.httpMethod` | 文字列 | サーバーへの呼び出しでAdobeが使用するメソッド。 オプションは`GET`、`PUT`、`POST`、`DELETE`、`PATCH`です。 |
| `httpTemplate.requestBody.templatingStrategy` | 文字列 | `PEBBLE_V1`.を使用します。 |
| `httpTemplate.requestBody.value` | 文字列 | この文字列は、Platformの顧客のデータをサービスが想定する形式に変換する文字エスケープバージョンです。<br> <ul><li> テンプレートの書き込み方法については、[テンプレートの使用](./message-format.md#using-templating)を参照してください。 </li><li> 文字のエスケープについて詳しくは、RFC JSON標準の7](https://tools.ietf.org/html/rfc8259#section-7)節を参照してください。[ </li><li> 単純な変換の例については、[プロファイル属性](./message-format.md#attributes)変換を参照してください。 </li></ul> |
| `httpTemplate.contentType` | 文字列 | サーバーが受け入れるコンテンツタイプ。 この値は`application/json`と考えられます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTPステータス200と、新しく作成された宛先サーバー設定の詳細を返します。

## 宛先サーバーの設定のリスト {#retrieve-list}

IMS組織の宛先サーバー設定のリストを取得するには、`/authoring/destination-servers`エンドポイントにGETリクエストを送信します。

**API 形式**


```http
GET /authoring/destination-servers
```

**リクエスト**

次のリクエストは、IMS組織とサンドボックス設定に基づいて、アクセス権のある宛先サーバー設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用したIMS組織IDとサンドボックス名に基づいて、HTTPステータス200と、アクセス権のある宛先サーバー設定のリストを返します。 1つの`instanceId`は、1つの宛先サーバーのテンプレートに対応します。 簡潔にするために応答は切り捨てられます。

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

`/authoring/destination-servers`エンドポイントにPUTリクエストを送信し、更新する宛先サーバー設定のインスタンスIDを指定することで、既存の宛先サーバー設定を更新できます。 呼び出しの本文で、更新された宛先サーバーの設定を指定します。

**API 形式**


```http
PUT /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先サーバー設定のID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された、既存の宛先サーバーの設定を更新します。

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

`/authoring/destination-servers`エンドポイントにGETリクエストを送信し、更新する宛先サーバー設定のインスタンスIDを指定することで、特定の宛先サーバー設定に関する詳細な情報を取得できます。

**API 形式**


```http
GET /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する宛先サーバー設定のID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destination-servers/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTPステータス200と、指定された宛先サーバー設定に関する詳細情報を返します。

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

`/authoring/destination-servers`エンドポイントにDELETEリクエストを送信し、削除する宛先サーバー設定のIDをリクエストパスに指定することで、指定した宛先サーバー設定を削除できます。

**API 形式**

```http
DELETE /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する宛先サーバー設定の`id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/destination-servers/bd4ec8f0-e98f-4b6a-8064-dd7adbfffec9 \
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

このドキュメントを読むと、`/authoring/destination-servers` APIエンドポイントを使用して宛先サーバーとテンプレートを設定する方法がわかります。 [宛先SDKを使用して宛先](./configure-destination-instructions.md)を設定する方法を読み、この手順が宛先の設定プロセスにどのように適合するかを理解してください。
