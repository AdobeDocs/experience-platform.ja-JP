---
description: Adobeは、Destination SDKの一環として、宛先の設定とテストを支援する開発者ツールを提供します。 このページでは、宛先設定のテスト方法について説明します。
title: 宛先設定のテスト
exl-id: 21e4d647-1168-4cb4-a2f8-22d201e39bba
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 3%

---

# 宛先設定のテスト {#developer-tools}

## 概要 {#overview}

Adobeは、Destination SDKの一環として、宛先の設定とテストを支援する開発者ツールを提供します。 このページでは、宛先設定のテスト方法について説明します。 メッセージ変換テンプレートの作成方法について詳しくは、 [メッセージ変換テンプレートの作成とテスト](./create-template.md).

宛先 **宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証します。**、 *宛先テストツール*. このツールを使用して、REST API エンドポイントにメッセージを送信することで、宛先設定をテストできます。

次の図に、宛先のテストがにどのように適合するかを示します [宛先設定ワークフロー](./configure-destination-instructions.md) Destination SDK:

![宛先のテスト手順が宛先設定ワークフローに適合する場所のグラフィック](./assets/test-destination-step.png)

## 宛先テストツール — 目的と前提条件 {#destination-testing-tool}

宛先テストツールを使用して、 [サーバー設定](./server-and-template-configuration.md).

ツールを使用する前に、次の点を確認します。
* 宛先を設定するには、 [宛先設定ワークフロー](./configure-destination-instructions.md) および
* 宛先への接続を確立します ( [宛先インスタンス ID の取得方法](./destination-testing-api.md#get-destination-instance-id).

このツールを使用すると、宛先を設定した後、次の操作を実行できます。
* 宛先が正しく設定されているかどうかをテストする。
* 設定した宛先へのデータフローの整合性を検証します。

### 使用方法 {#how-to-use}

>[!NOTE]
>
>API リファレンスのドキュメントについて詳しくは、 [宛先テスト API 操作](./destination-testing-api.md).

宛先テスト API エンドポイントへの呼び出しは、リクエストにプロファイルを追加する場合も、追加しない場合もおこなえます。

リクエストにプロファイルを追加しない場合、Adobeは内部的にプロファイルを生成し、リクエストに追加します。 このリクエストで使用するプロファイルを生成する場合は、 [サンプルのプロファイル生成 API リファレンス](./sample-profile-generation-api.md). ソース XDM スキーマに基づいてプロファイルを生成する必要があります ( [API リファレンス](./sample-profile-generation-api.md#generate-sample-profiles-source-schema). ソーススキーマは [和集合スキーマ](https://experienceleague.adobe.com/docs/experience-platform/profile/union-schemas/union-schema.html?lang=en) 使用するサンドボックスの。

応答には、宛先リクエストの処理結果が含まれます。 リクエストには、次の 3 つの主なセクションが含まれます。
* 宛先のAdobeで生成されたリクエスト。
* 宛先から受信した応答。
* リクエストで送信されたプロファイルのリスト ( プロファイルが [をリクエストに追加しました](./destination-testing-api.md/#test-with-added-profiles)、または次の場合にAdobeで生成 [宛先テストリクエストの本文が空でした](./destination-testing-api.md#test-without-adding-profiles).

>[!NOTE]
>
>Adobeは、複数のリクエストと応答のペアを生成する場合があります。 例えば、 `maxUsersPerRequest` 値が 7 の場合、1 つのリクエストが 7 つのプロファイルを持ち、もう 1 つのリクエストが 3 つのプロファイルを持ちます。

**本文に profiles パラメーターを含むリクエストのサンプル**

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/3e0ac39c-ef14-4101-9fd9-cf0909814510' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
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
            "Email":[
               {
                  "id":"Email-iIyJc"
               }
            ],
            "IDFA":[
               {
                  "id":"IDFA-viPAW"
               }
            ],
            "GAID":[
               {
                  "id":"GAID-Bc6LE"
               }
            ],
            "Email_LC_SHA256":[
               {
                  "id":"Email_LC_SHA256-gEOdj"
               }
            ]
         },
         "attributes":{
            "key":{
               "value":"string"
            }
         }
      }
   ]
}'
```

**本文にプロファイルパラメーターがないリクエストのサンプル**


```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/3e0ac39c-ef14-4101-9fd9-cf0909814510' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw ''
```

**レスポンスのサンプル**

なお、 `results.httpCalls` パラメーターは、REST API に固有です。

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
            "Email":[
               {
                  "id":"Email-iIyJc"
               }
            ],
            "IDFA":[
               {
                  "id":"IDFA-viPAW"
               }
            ],
            "GAID":[
               {
                  "id":"GAID-Bc6LE"
               }
            ],
            "Email_LC_SHA256":[
               {
                  "id":"Email_LC_SHA256-gEOdj"
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

リクエストパラメーターと応答パラメーターについて詳しくは、 [宛先テスト API 操作](./destination-testing-api.md).

## 次の手順

宛先をテストし、正しく設定されていることを確認したら、 [宛先公開 API](./destination-publish-api.md) 設定をレビュー用にAdobeに送信します。
