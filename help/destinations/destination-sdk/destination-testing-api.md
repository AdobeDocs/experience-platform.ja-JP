---
description: このページでは、「/authoring/testing/destinationInstance/」 API エンドポイントを使用して実行できる、宛先が正しく設定されているかどうかのテストおよび設定した宛先へのデータフローの整合性の検証をおこなうための API 操作の一覧と説明を示します。
title: 宛先テスト API 操作
exl-id: 2b54250d-ec30-4ad7-a8be-b86b14e4f074
source-git-commit: 6dd8a94e46b9bee6d1407e7ec945a722d8d7ecdb
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 2%

---

# 宛先テスト API 操作 {#template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/`

このページでは、 `/authoring/testing/destinationInstance/` API エンドポイント：宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証します。 このエンドポイントでサポートされる機能については、 [宛先設定のテスト](./test-destination.md).

呼び出しにプロファイルを追加するかどうかに関わらず、テストエンドポイントにリクエストをおこないます。 リクエストでプロファイルを送信しない場合、Adobeは内部でプロファイルを生成し、リクエストに追加します。

以下を使用して、 [サンプルプロファイル生成 API](./sample-profile-generation-api.md) を使用して、宛先テスト API へのリクエストで使用するプロファイルを作成します。

## 宛先インスタンス ID の取得方法 {#get-destination-instance-id}

>[!IMPORTANT]
>
>* この API を使用するには、Experience PlatformUI で宛先への既存の接続が必要です。 読み取り [宛先に接続](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=en) および [宛先へのプロファイルとセグメントのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en) を参照してください。 宛先への接続を確立したら、このエンドポイントへの API 呼び出しで使用する必要がある宛先インスタンス ID を、次の場合に URL から取得します。 [宛先との接続の参照](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/destination-details-page.html?lang=en).
   >![UI 画像宛先インスタンス ID の取得方法](./assets/get-destination-instance-id.png)


## 宛先テスト API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## 呼び出しにプロファイルを追加せずに、宛先設定をテストする {#test-without-adding-profiles}

宛先の設定をテストするには、 `authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}` エンドポイントを作成し、テストする宛先の宛先インスタンス ID を指定します。

**API 形式**


```http
POST authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}
```

| クエリパラメータ | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | テストする宛先の宛先インスタンス ID。 |

**リクエスト**

次のリクエストは、宛先の REST API エンドポイントを呼び出します。 リクエストは `{DESTINATION_INSTANCE_ID}` クエリパラメーター。

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/49966037-32cd-4457-a105-2cbf9c01826a' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と共に、宛先の REST API エンドポイントからの API 応答を返します。

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
| `aggregationKey` | 宛先に対して設定された集計ポリシーに関する情報が含まれます。 詳しくは、 [集計ポリシー](./destination-configuration.md#aggregation) の節を参照してください。 |
| `traceId` | 操作の一意の ID。 エラーが発生した場合は、この ID をトラブルシューティング用にAdobeチームと共有できます。 |
| `results.httpCalls.request` | 宛先にAdobeで送信されたリクエストを含めます。 |
| `results.httpCalls.response` | 宛先からAdobeが受け取った応答を含みます。 |
| `inputProfiles` | 宛先への呼び出し時に書き出されたプロファイルが含まれます。 プロファイルは、ソーススキーマと一致します。 |

{style=&quot;table-layout:auto&quot;}

## 呼び出しに追加されたプロファイルを使用して、宛先設定をテストする {#test-with-added-profiles}

宛先の設定をテストするには、 `authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}` エンドポイントを作成し、テストする宛先の宛先インスタンス ID を指定します。

**API 形式**

```http
POST authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}
```

| クエリパラメータ | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | テストする宛先の宛先インスタンス ID。 |

**リクエスト**

次のリクエストは、宛先の REST API エンドポイントを呼び出します。 リクエストは、ペイロードで指定されたパラメーターと `{DESTINATION_INSTANCE_ID}` クエリパラメーター。

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/49966037-32cd-4457-a105-2cbf9c01826a' \
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

正常な応答は、HTTP ステータス 200 と共に、宛先の REST API エンドポイントからの API 応答を返します。

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

## API エラー処理 {#api-error-handling}

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順

このドキュメントを読んだ後、宛先のテスト方法がわかりました。 これで、Adobe [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md) をクリックして、宛先のドキュメントページを作成します。
