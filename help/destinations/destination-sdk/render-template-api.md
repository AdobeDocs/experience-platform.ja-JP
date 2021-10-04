---
description: このページでは、メッセージ変換テンプレートに基づいて、「/authoring/testing/template/render」 API エンドポイントを使用して、宛先に書き出したデータをレンダリングするために実行できるすべての API 操作について説明します。
title: レンダリングテンプレート API 操作
exl-id: e64ea89e-6064-4a05-9730-e0f7d7a3e1db
source-git-commit: 9be8636b02a15c8f16499172289413bc8fb5b6f0
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 2%

---

# レンダリングテンプレート API 操作 {#render-template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/testing/template/render`

このページでは、`/authoring/testing/template/render` API エンドポイントを使用して、[ メッセージ変換テンプレート ](./message-format.md#using-templating) に基づいて、宛先に書き出されたデータをレンダリングするために実行できるすべての API 操作について説明します。 このエンドポイントでサポートされる機能の説明については、[ テンプレートの作成 ](./create-template.md) を参照してください。

## レンダリングテンプレート API 操作の概要 {#get-started}

続行する前に、[ はじめに ](./getting-started.md) を参照して、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、API を正しく呼び出すために知っておく必要がある重要な情報を確認してください。

## 書き出されたデータをテンプレートに基づいてレンダリング {#render-exported-data}

`authoring/testing/template/render` エンドポイントにPOSTリクエストを送信し、[ サンプルテンプレート API エンドポイント ](./sample-template-api.md) を使用して作成した宛先設定の宛先 ID とテンプレートを指定することで、書き出されたデータをレンダリングできます。

>[!TIP]
>
>* ここで使用する宛先 ID は、`/destinations` エンドポイントを使用して作成された、宛先設定に対応する `instanceId` です。 [ 宛先設定 API リファレンス ](./destination-configuration-api.md#retrieve-list) を参照してください。



**API 形式**


```http
POST authoring/testing/template/render
```

| パラメーター | 説明 |
| -------- | ----------- |
| `destinationId` | 書き出したデータをレンダリングする宛先設定の ID。 |
| `template` | 書き出されたデータをレンダリングする際に基づくテンプレートの文字エスケープバージョン。 |
| `profiles` | 呼び出しの本文にプロファイルを追加する場合は、[ サンプルプロファイル生成 API](./sample-profile-generation-api.md) を使用してプロファイルを生成できます。 |

{style=&quot;table-layout:auto&quot;}


次の例に示すように、書き出されたデータをレンダリングできます。

* [本文でプロファイルを送信しないテンプレートのレンダリング](./render-template-api.md#multiple-profiles-no-body)
* [本文で送信したプロファイルを使用したテンプレートのレンダリング](./render-template-api.md#multiple-profiles-with-body)

<!--

### Render a template for single profile, no profiles sent in body {#single-profile-no-body}

**Request**

The following request renders sample profiles that match the format expected by your destination.

```shell

curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '
{
    "destinationId": "a9fb4f0f-3983-4756-b756-00cf25fe5a35",
    "template": "{# THIS is an example template for a single profile #}\r\n{\r\n    \"identities\": [\r\n        {% for email in input.profile.identityMap.email %}\r\n            {\r\n                \"type\": \"email\",\r\n                \"id\": \"{{ email.id }}\"\r\n            }{% if not loop.last %},{% endif %}\r\n        {% endfor %}\r\n\r\n        {# Add a comma only if we have both emails and external_ids. #}\r\n        {% if input.profile.identityMap.email is not empty and input.profile.identityMap.external_id is not empty %}\r\n            ,\r\n        {% endif %}\r\n\r\n        {% for external in input.profile.identityMap.external_id %}\r\n            {\r\n                \"type\": \"external_id\",\r\n                \"id\": \"{{ external.id }}\"\r\n            }{% if not loop.last %},{% endif %}\r\n        {% endfor %}\r\n    ],\r\n    \"AdobeExperiencePlatformSegments\": {\r\n        \"add\": [\r\n            {% for segment in input.profile.segmentMembership.ups | added %}\r\n                \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n            {% endfor %}\r\n        ],\r\n        \"remove\": [\r\n            {# Alternative syntax for filtering segments by status: #}\r\n            {% for segment in removedSegments(input.profile.segmentMembership.ups) %}\r\n                \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n            {% endfor %}\r\n        ]\r\n    }\r\n}"
}'

```

**Response**

The response returns the result of rendering the template, or any errors encountered.
A successful response returns HTTP status 200 with details of the exported data.
An unsuccessful response returns HTTP status 500 along with descriptions of the encountered errors.

```json

{
    "identities": [
        {
            "type": "email",
            "id": "email_lc_sha256-cBltJ"
        },
        {
            "type": "external_id",
            "id": "extern_id-cP732"
        }
    ],
    "AdobeExperiencePlatformSegments": {
        "add": [
            "segmentid1",
            "segmentid2"
        ],
        "remove": [
            "segmentid3"
        ]
    }
}

```

### Render a template for single profile, with profiles sent in body {#single-profile-with-body}

**Request**

The following request renders a profile that matches the format expected by your destination. You can generate profiles to send on the call by using the [sample profile generation API](./sample-profile-generation-api.md).

```shell

curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '
{
    "destinationId": "a9fb4f0f-3983-4756-b756-00cf25fe5a35",
    "template": "{# THIS is an example template for a single profile #}\r\n{\r\n    \"identities\": [\r\n        {% for email in input.profile.identityMap.email %}\r\n            {\r\n                \"type\": \"email\",\r\n                \"id\": \"{{ email.id }}\"\r\n            }{% if not loop.last %},{% endif %}\r\n        {% endfor %}\r\n\r\n        {# Add a comma only if we have both emails and external_ids. #}\r\n        {% if input.profile.identityMap.email is not empty and input.profile.identityMap.external_id is not empty %}\r\n            ,\r\n        {% endif %}\r\n\r\n        {% for external in input.profile.identityMap.external_id %}\r\n            {\r\n                \"type\": \"external_id\",\r\n                \"id\": \"{{ external.id }}\"\r\n            }{% if not loop.last %},{% endif %}\r\n        {% endfor %}\r\n    ],\r\n    \"AdobeExperiencePlatformSegments\": {\r\n        \"add\": [\r\n            {% for segment in input.profile.segmentMembership.ups | added %}\r\n                \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n            {% endfor %}\r\n        ],\r\n        \"remove\": [\r\n            {# Alternative syntax for filtering segments by status: #}\r\n            {% for segment in removedSegments(input.profile.segmentMembership.ups) %}\r\n                \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n            {% endfor %}\r\n        ]\r\n    }\r\n}",
    "profiles": [
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-17T11:49:06.626238Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-17T11:49:06.626240Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-17T11:49:06.626240Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-c3fjU"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-VNq0z"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-HATAl"
                }
            ],
            "external_id": [
                {
                    "id": "extern_id-cP732"
                }
            ],
            "email": [
                {
                    "id": "email_lc_sha256-cBltJ"
                }
            ]
        }
    }
    ]
}'

```

**Response**

A successful response returns HTTP status 200 with details of the exported data.

```json

{
    "identities": [
        {
            "type": "email",
            "id": "email_lc_sha256-cBltJ"
        },
        {
            "type": "external_id",
            "id": "extern_id-cP732"
        }
    ],
    "AdobeExperiencePlatformSegments": {
        "add": [
            "segmentid1",
            "segmentid2"
        ],
        "remove": [
            "segmentid3"
        ]
    }
}

```

-->

### 本文でプロファイルを送信しないテンプレートのレンダリング {#multiple-profiles-no-body}

**リクエスト**

次のリクエストでは、宛先で想定されている形式に一致する複数のサンプルプロファイルがレンダリングされます。

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '
{
   "destinationId":"{DESTINATION_ID}",
   "template":"{# THIS is an example template for multiple profiles #}\r\n{\r\n    \"profiles\": [\r\n        {% for profile in input.profiles %}\r\n            {\r\n                \"identities\": [\r\n                    {% for email in profile.identityMap.email %}\r\n                        {\r\n                            \"type\": \"email\",\r\n                            \"id\": \"{{ email.id }}\"\r\n                        }{% if not loop.last %},{% endif %}\r\n                    {% endfor %}\r\n\r\n                    {# Add a comma only if we have both emails and external_ids. #}\r\n                    {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}\r\n                        ,\r\n                    {% endif %}\r\n\r\n                    {% for external in profile.identityMap.external_id %}\r\n                        {\r\n                            \"type\": \"external_id\",\r\n                            \"id\": \"{{ external.id }}\"\r\n                        }{% if not loop.last %},{% endif %}\r\n                    {% endfor %}\r\n                ],\r\n                \"AdobeExperiencePlatformSegments\": {\r\n                    \"add\": [\r\n                        {% for segment in profile.segmentMembership.ups | added %}\r\n                            \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n                        {% endfor %}\r\n                    ],\r\n                    \"remove\": [\r\n                        {# Alternative syntax for filtering segments by status: #}\r\n                        {% for segment in removedSegments(profile.segmentMembership.ups) %}\r\n                            \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n                        {% endfor %}\r\n                    ]\r\n                }\r\n            }{% if not loop.last %},{% endif %}\r\n        {% endfor %}\r\n    ]\r\n}",
   "profiles":[
      {
         "segmentMembership":{
            "ups":{
               "segmentid1":{
                  "lastQualificationTime":"2021-06-17T12:08:07.870859Z",
                  "status":"existing"
               },
               "segmentid3":{
                  "lastQualificationTime":"2021-06-17T12:08:07.870860Z",
                  "status":"exited"
               },
               "segmentid2":{
                  "lastQualificationTime":"2021-06-17T12:08:07.870860Z",
                  "status":"realized"
               }
            }
         },
         "identityMap":{
            "email":[
               {
                  "id":"Email-gq3zZ"
               }
            ],
            "external_id":[
               {
                  "id":"external_id"
               }
            ]
         }
      }
   ]
}'
```

**応答**

応答は、テンプレートのレンダリングの結果、または発生したエラーを返します。
正常な応答は、HTTP ステータス 200 と、エクスポートされたデータの詳細を返します。
失敗した応答は、HTTP ステータス 500 と、発生したエラーの説明を返します。

```json
{
   "profiles":[
      {
         "identities":[
            
         ],
         "AdobeExperiencePlatformSegments":{
            "add":[
               "segmentid1",
               "segmentid2"
            ],
            "remove":[
               "segmentid3"
            ]
         }
      },
      {
         "identities":[
            
         ],
         "AdobeExperiencePlatformSegments":{
            "add":[
               "segmentid1",
               "segmentid2"
            ],
            "remove":[
               "segmentid3"
            ]
         }
      },
      {
         "identities":[
            
         ],
         "AdobeExperiencePlatformSegments":{
            "add":[
               "segmentid1",
               "segmentid2"
            ],
            "remove":[
               "segmentid3"
            ]
         }
      }
   ]
}       
```

### 本文で送信したプロファイルを使用したテンプレートのレンダリング {#multiple-profiles-with-body}

**リクエスト**

次のリクエストでは、宛先で想定されている形式に一致するサンプルプロファイルがレンダリングされます。 [ サンプルプロファイル生成 API](./sample-profile-generation-api.md) を使用して、呼び出し時に送信するプロファイルを生成できます。

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '
{
   "destinationId":"{DESTINATION_ID}",
   "template":"{# THIS is an example template for multiple profiles #}\r\n{\r\n    \"profiles\": [\r\n        {% for profile in input.profiles %}\r\n            {\r\n                \"identities\": [\r\n                    {% for email in profile.identityMap.email %}\r\n                        {\r\n                            \"type\": \"email\",\r\n                            \"id\": \"{{ email.id }}\"\r\n                        }{% if not loop.last %},{% endif %}\r\n                    {% endfor %}\r\n\r\n                    {# Add a comma only if we have both emails and external_ids. #}\r\n                    {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}\r\n                        ,\r\n                    {% endif %}\r\n\r\n                    {% for external in profile.identityMap.external_id %}\r\n                        {\r\n                            \"type\": \"external_id\",\r\n                            \"id\": \"{{ external.id }}\"\r\n                        }{% if not loop.last %},{% endif %}\r\n                    {% endfor %}\r\n                ],\r\n                \"AdobeExperiencePlatformSegments\": {\r\n                    \"add\": [\r\n                        {% for segment in profile.segmentMembership.ups | added %}\r\n                            \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n                        {% endfor %}\r\n                    ],\r\n                    \"remove\": [\r\n                        {# Alternative syntax for filtering segments by status: #}\r\n                        {% for segment in removedSegments(profile.segmentMembership.ups) %}\r\n                            \"{{ segment.key }}\"{% if not loop.last %},{% endif %}\r\n                        {% endfor %}\r\n                    ]\r\n                }\r\n            }{% if not loop.last %},{% endif %}\r\n        {% endfor %}\r\n    ]\r\n}",
   "profiles":[
      {
         "segmentMembership":{
            "ups":{
               "segmentid1":{
                  "lastQualificationTime":"2021-06-17T12:08:07.870859Z",
                  "status":"existing"
               },
               "segmentid3":{
                  "lastQualificationTime":"2021-06-17T12:08:07.870860Z",
                  "status":"exited"
               },
               "segmentid2":{
                  "lastQualificationTime":"2021-06-17T12:08:07.870860Z",
                  "status":"realized"
               }
            }
         },
         "identityMap":{
            "email":[
               {
                  "id":"Email-gq3zZ"
               }
            ],
            "external_id":[
               {
                  "id":"external_id"
               }
            ]
         }
      }
   ]
}'
```

**応答**

応答は、テンプレートのレンダリングの結果、または発生したエラーを返します。
正常な応答は、HTTP ステータス 200 と、エクスポートされたデータの詳細を返します。
失敗した応答は、HTTP ステータス 500 と、発生したエラーの説明を返します。

```json
{
   "profiles":[
      {
         "identities":[
            {
               "type":"email",
               "id":"Email-gq3zZ"
            },
            {
               "type":"external_id",
               "id":"external_id"
            }
         ],
         "AdobeExperiencePlatformSegments":{
            "add":[
               "segmentid1",
               "segmentid2"
            ],
            "remove":[
               "segmentid3"
            ]
         }
      }
   ]
}
```

## API エラー処理 {#api-error-handling}

宛先 SDK API エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 Platform トラブルシューティングガイドの [API ステータスコード ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes) および [ リクエストヘッダーのエラー ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読むと、メッセージ変換テンプレートを使用して、宛先で予想されるデータ形式に一致する書き出し済みのプロファイルを生成する方法がわかります。 [ 宛先 SDK を使用して宛先を設定する方法 ](./configure-destination-instructions.md) を読んで、この手順が宛先の設定プロセスにどのように適しているかを確認してください。
