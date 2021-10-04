---
description: 宛先 SDK の一部として、Adobeには、宛先の設定とテストを支援する開発者ツールが用意されています。 このページでは、宛先設定をテストする方法について説明します。
title: 宛先設定のテスト
exl-id: 21e4d647-1168-4cb4-a2f8-22d201e39bba
source-git-commit: 3d7151645bc90a2dcbd6b31251ed459029ab77c9
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 1%

---

# 宛先設定のテスト {#developer-tools}

## 概要 {#overview}

宛先 SDK の一部として、Adobeには、宛先の設定とテストを支援する開発者ツールが用意されています。 このページでは、宛先設定をテストする方法について説明します。 メッセージ変換テンプレートの作成方法については、「[ メッセージ変換テンプレートの作成とテスト ](./create-template.md)」を参照してください。

**宛先が正しく設定されているかどうかをテストし、設定した宛先へのデータフローの整合性を検証するには**、*宛先テストツール* を使用します。 このツールを使用すると、REST API エンドポイントにメッセージを送信して、宛先の設定をテストできます。

次の図に、宛先 SDK の [ 宛先設定ワークフロー ](./configure-destination-instructions.md) に宛先のテストがどのように適合するかを示します。

![宛先テスト手順が宛先設定ワークフローに適合する場所の図](./assets/test-destination-step.png)

## 宛先テストツール {#destination-testing-tool}

このツールを使用して、[ サーバー設定 ](./server-and-template-configuration.md) で指定したパートナーエンドポイントにメッセージを送信し、宛先設定をテストします。

このツールを使用すると、宛先を設定した後、次の操作を実行できます。
* 宛先が正しく設定されているかどうかをテストする。
* 設定した宛先へのデータフローの整合性を検証します。

### 使用方法 {#how-to-use}

>[!NOTE]
>
>API リファレンスのドキュメントについて詳しくは、[ 宛先テスト API 操作 ](./destination-testing-api.md) を参照してください。

宛先テスト API エンドポイントへの呼び出しは、リクエストにプロファイルを追加する場合と、追加しない場合のどちらでおこなうこともできます。

リクエストにプロファイルを追加しない場合、Adobeは内部でプロファイルを生成し、リクエストに追加します。 このリクエストで使用するプロファイルを生成する場合は、[ サンプルのプロファイル生成 API リファレンス ](./sample-profile-generation-api.md) を参照してください。 [API リファレンス ](./sample-profile-generation-api.md#generate-sample-profiles-source-schema) に示すように、ソース XDM スキーマに基づいてプロファイルを生成する必要があります。 ソーススキーマは、使用しているサンドボックスの [ 和集合スキーマ ](https://experienceleague.adobe.com/docs/experience-platform/profile/union-schemas/union-schema.html?lang=en) です。

応答には、宛先リクエスト処理の結果が含まれます。 リクエストには、次の 3 つの主なセクションが含まれます。
* 宛先に対してAdobeによって生成された要求。
* 宛先から受信した応答。
* リクエストで送信されたプロファイルのリスト。リクエスト ](./destination-testing-api.md/#test-with-added-profiles) で追加されたプロファイルか、宛先テストリクエストの本文が空の場合はAdobeによって生成されたプロファイルか。[[](./destination-testing-api.md#test-without-adding-profiles)

>[!NOTE]
>
>Adobeは、複数のリクエストと応答のペアを生成できます。 例えば、値が 7 の宛先に 10 件のプロファイルを送信した場合、1 件のリクエストに 7 件のプロファイルが送信され、もう 1 件のリクエストに 3 件のプロファイルが送信されます。`maxUsersPerRequest`

**本文にプロファイルパラメーターを含むリクエストのサンプル**

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/3e0ac39c-ef14-4101-9fd9-cf0909814510' \
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

**本文にプロファイルパラメーターを含まないリクエストのサンプル**


```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/3e0ac39c-ef14-4101-9fd9-cf0909814510' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw ''
```

**レスポンスのサンプル**

`results.httpCalls` パラメーターの内容は REST API に固有です。

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

リクエストパラメーターと応答パラメーターについて詳しくは、[ 宛先テスト API の操作 ](./destination-testing-api.md) を参照してください。

## 次の手順

宛先をテストし、正しく設定されていることを確認したら、[ 宛先パブリッシング API](./destination-publish-api.md) を使用して、設定をAdobeに送信し、確認します。
