---
description: このページでは、「/authoring/testing/destinationInstance/」 APIエンドポイントを使用して実行できるすべてのAPI操作の一覧と説明を示し、宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証します。
title: 宛先テストAPI操作
source-git-commit: 19307fba8f722babe5b6d57e80735ffde00fc851
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 2%

---

# 宛先テストAPI操作 {#template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/`

このページでは、`/authoring/testing/destinationInstance/` APIエンドポイントを使用して実行できるすべてのAPI操作について説明し、宛先が正しく設定されているかどうかをテストし、設定済みの宛先へのデータフローの整合性を検証します。 このエンドポイントでサポートされる機能の説明については、[宛先設定のテスト](./test-destination.md)を参照してください。

呼び出しにプロファイルを追加するかどうかにかかわらず、テストエンドポイントにリクエストをおこないます。 リクエストでプロファイルを送信しない場合、Adobeは内部でプロファイルを生成し、リクエストに追加します。

[サンプルのプロファイル生成API](./sample-profile-generation-api.md)を使用して、宛先テストAPIへのリクエストで使用するプロファイルを作成できます。

>[!IMPORTANT]
>
>* このAPIを使用するには、Experience PlatformUIで宛先への既存の接続が必要です。 詳しくは、[宛先への接続](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=en)および[宛先へのプロファイルとセグメントのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en)をお読みください。 宛先への接続を確立したら、[宛先](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/destination-details-page.html?lang=en)との接続を参照する際に、URLからこのエンドポイントへのAPI呼び出しで使用する必要がある宛先インスタンスIDを取得します。
   >![UI画像宛先インスタンスIDの取得方法](./assets/get-destination-instance-id.png)


## 宛先テストAPI操作の概要 {#get-started}

続行する前に、[はじめに](./getting-started.md)を参照し、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 呼び出しにプロファイルを追加せずに、宛先設定をテストする {#test-without-adding-profiles}

`authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}`エンドポイントにPOSTリクエストを送信し、テストする宛先の宛先インスタンスIDを指定して、宛先の設定をテストできます。

**API 形式**


```http
POST authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | テストする宛先の宛先インスタンスID。 |

**リクエスト**

次のリクエストは、宛先のREST APIエンドポイントを呼び出します。 リクエストは`{DESTINATION_INSTANCE_ID}`クエリパラメーターで設定します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/49966037-32cd-4457-a105-2cbf9c01826a' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTPステータス200と共に、宛先のREST APIエンドポイントからのAPI応答を返します。

```json
{
   "results":[
      {
         "aggregationKey":{
            "destinationInstanceId":"string",
            "segmentId":"string",
            "segmentStatus":"realized",
            "identityNamespaces":[
               [
                  "email",
                  "phone"
               ]
            ]
         },
         "httpCalls":[
            {
               "traceId":"a06fec2d-a886-4219-8975-4e4b7ed26539",
               "request":{
                  "body":"{ \"attributes\": [  { \"external_id\": \"external_id-h29Fq\"  , \"AdobeExperiencePlatformSegments\": { \"add\": [  \"Nirvana fans\" ,  \"RHCP fans\"   ], \"remove\": [  ] }  ,  \"key\":  \"string\"    }  ] }",
                  "headers":[
                     {
                        "Content-Type":"application/json"
                     }
                  ],
                  "method":"POST",
                  "uri":"https://api.moviestar.com/users/track"
               },
               "response":{
                  "body":"{\"status\": \"success\"}",
                  "code":"200",
                  "headers":[
                     {
                        "Connection":"keep-alive"
                     },
                     {
                        "Content-Type":"application/json"
                     },
                     {
                        "Server":"nginx"
                     },
                     {
                        "Vary":"Origin,Accept-Encoding"
                     },
                     {
                        "transfer-encoding":"chunked"
                     }
                  ]
               }
            }
         ]
      }
   ],
   "inputProfiles":[
      {
         "segmentMembership":{
            "ups":{
               "03fb9938-8537-4b4c-87f9-9c4d413a0ee5":{
                  "lastQualificationTime":"2021-06-17T12:25:12.872039Z",
                  "status":"realized"
               },
               "27e05542-d6a3-46c7-9c8e-d59d50229530":{
                  "lastQualificationTime":"2021-06-17T12:25:12.872042Z",
                  "status":"realized"
               }
            }
         },
         "personalEmail":{
            "address":"john.smith@abc.com"
         },
         "identityMap":{
            "ECID":[
               {
                  "id":"ECID-vlnt6"
               }
            ]
         },
         "person":{
            "name":{
               "firstName":"string"
            }
         }
      }
   ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `aggregationKey` | 宛先に対して設定された集計ポリシーに関する情報が含まれます。 詳しくは、宛先の設定ドキュメントの「[集計ポリシー](./destination-configuration.md#aggregation)」の節を参照してください。 |
| `traceId` | 操作の一意の識別子。 エラーが発生した場合は、このIDをトラブルシューティング用にAdobeチームと共有できます。 |
| `results.httpCalls.request` | 宛先に送信されたリクエストがAdobeに含まれます。 |
| `results.httpCalls.response` | 宛先から受信したAdobeに含まれます。 |
| `inputProfiles` | 宛先への呼び出し時に書き出されたプロファイルが含まれます。 プロファイルは、ソーススキーマと一致します。 |


## 呼び出しに追加されたプロファイルを使用して、宛先設定をテストする {#test-with-added-profiles}

`authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}`エンドポイントにPOSTリクエストを送信し、テストする宛先の宛先インスタンスIDを指定して、宛先の設定をテストできます。

**API 形式**

```http
POST authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | テストする宛先の宛先インスタンスID。 |

**リクエスト**

次のリクエストは、宛先のREST APIエンドポイントを呼び出します。 リクエストは、ペイロードで指定されたパラメーターと`{DESTINATION_INSTANCE_ID}`クエリパラメーターによって設定されます。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/49966037-32cd-4457-a105-2cbf9c01826a' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '{
   "profiles":[
      {
         "segmentMembership":{
            "ups":{
               "374a9a6c-c719-4cdb-a660-155a2838e6d6":{
                  "lastQualificationTime":"2021-05-13T12:16:27.248585Z",
                  "status":"realized"
               },
               "896f8776-9498-47b4-b994-51cb3f61c2c5":{
                  "lastQualificationTime":"2021-05-13T12:16:27.248605Z",
                  "status":"realized"
               }
            }
         },
         "identityMap":{
            "ECID":[
               {
                  "id":"ECID-Z3i2t"
               }
            ],
            "external_id":[
               {
                  "id":"external_id-h29Fq"
               }
            ]
         },
         "attributes":{
            "firstName":{
               "value":"John"
            }
         }
      }
   ]
}'
```

**応答**

正常な応答は、HTTPステータス200と共に、宛先のREST APIエンドポイントからのAPI応答を返します。

```json
{
   "results":[
      {
         "aggregationKey":{
            "destinationInstanceId":"string",
            "segmentId":"string",
            "segmentStatus":"realized",
            "identityNamespaces":[
               [
                  "email",
                  "phone"
               ]
            ]
         },
         "httpCalls":[
            {
               "traceId":"a06fec2d-a886-4219-8975-4e4b7ed26539",
               "request":{
                  "body":"{ \"attributes\": [  { \"external_id\": \"external_id-h29Fq\"  , \"AdobeExperiencePlatformSegments\": { \"add\": [  \"Nirvana fans\" ,  \"RHCP fans\"   ], \"remove\": [  ] }  ,  \"key\":  \"string\"    }  ] }",
                  "headers":[
                     {
                        "Content-Type":"application/json"
                     }
                  ],
                  "method":"POST",
                  "uri":"https://api.moviestar.com/users/track"
               },
               "response":{
                  "body":"{\"status\": \"success\"}",
                  "code":"200",
                  "headers":[
                     {
                        "Connection":"keep-alive"
                     },
                     {
                        "Content-Type":"application/json"
                     },
                     {
                        "Server":"nginx"
                     },
                     {
                        "Vary":"Origin,Accept-Encoding"
                     },
                     {
                        "transfer-encoding":"chunked"
                     }
                  ]
               }
            }
         ]
      }
   ],
   "inputProfiles":[
      {
         "segmentMembership":{
            "ups":{
               "374a9a6c-c719-4cdb-a660-155a2838e6d6":{
                  "lastQualificationTime":"2021-05-13T12:16:27.248585Z",
                  "status":"realized"
               },
               "896f8776-9498-47b4-b994-51cb3f61c2c5":{
                  "lastQualificationTime":"2021-05-13T12:16:27.248605Z",
                  "status":"realized"
               }
            }
         },
         "identityMap":{
            "ECID":[
               {
                  "id":"ECID-Z3i2t"
               }
            ],
            "external_id":[
               {
                  "id":"external_id-h29Fq"
               }
            ]
         },
         "attributes":{
            "firstName":{
               "value":"John"
            }
         }
      }
   ]
}
```

## APIエラー処理 {#api-error-handling}

宛先SDK APIエンドポイントは、一般的なExperience PlatformAPIエラーメッセージの原則に従います。 Platformトラブルシューティングガイドの[APIステータスコード](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes)および[リクエストヘッダーエラー](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読むと、宛先のテスト方法がわかります。 これで、Adobe[セルフサービスドキュメントプロセス](./docs-framework/documentation-instructions.md)を使用して、宛先のドキュメントページを作成できます。