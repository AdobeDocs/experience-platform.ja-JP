---
description: このページでは、Destination SDKを使用して作成した宛先のレビュー用に送信する必要があるすべての情報を提供します。
title: Destination SDKで作成した宛先を確認用に送信
source-git-commit: bc77614eee6cc50d2ce6b14c1b228ed87f88f340
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---

# Destination SDKで作成した宛先を確認用に送信

## 概要 {#overview}

宛先をに公開する前に [Experience Platform先カタログ](/help/destinations/catalog/overview.md)を使用する場合は、Adobeに宛先とテストに関する特定の情報を提供し、プラットフォームにデータをアクティブ化する際に、ユーザーが可能な限り最高のエクスペリエンスを享受できるようにする必要があります。

このページには、Adobe Experience Platform Destination SDKを使用して作成した宛先を送信または更新する際に提供する必要があるすべての情報が一覧表示されます。 Adobe Experience Platformで宛先を正常に送信するには、に電子メールを送信します。 <aepdestsdk@adobe.com> 次を含みます。

* 宛先が解決する使用例の説明です。 既存の宛先設定を更新する場合は、これは必要ありません。
* 宛先への HTTP 呼び出しを実行するためにテスト宛先 API エンドポイントを使用した後の結果のテスト。 Adobe:
   * 宛先エンドポイントに対しておこなわれる API 呼び出し。
   * 宛先エンドポイントから受け取った API 応答。
* 宛先に対して、 [宛先公開 API](./destination-publish-api.md).
* （製品化された統合のみ）ドキュメントの PR（プル要求）。 [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md).
* 画像の宛先カタログ内の宛先カードのロゴとして表示されるExperience Platformファイル。

各項目の詳細については、以下の節を参照してください。

## 使用例の説明

宛先がExperience Platformのお客様に解決する使用例の説明を入力します。 説明は、既存のパートナーの使用例に似ています。

* [Pinterest](/help/destinations/catalog/advertising/pinterest.md):顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成します。
* [Yahoo データ X](/help/destinations/catalog/advertising/datax.md#use-cases):Verizon Media(VMG) の電子メールアドレスをキーにした特定のオーディエンスグループをターゲットにしたい広告主は、VMG のほぼリアルタイム API を使用して、新しいセグメントをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## テスト宛先 API を使用した後の結果のテスト

を使用した後にテスト結果を提供する [宛先 API のテスト](./test-destination.md) エンドポイントを使用して、宛先への HTTP 呼び出しを実行します。 これには以下が含まれます。
* テスト API を使用して宛先エンドポイントに対しておこなわれた完全な API リクエスト（ヘッダーと本文）。
* 宛先エンドポイントから受け取った API 応答。

例えば、リクエストと応答は以下のサンプルのようになります。

**リクエスト**

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

**応答**

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

## 宛先の公開リクエストを送信した配達確認

宛先を正常にテストした後は、 [宛先公開 API](./destination-publish-api.md) をクリックして、レビューおよび公開用にAdobeに宛先を送信します。

宛先の公開リクエストの ID を指定します。 公開リクエスト ID の取得方法について詳しくは、 [宛先の公開リクエストのリスト](./destination-publish-api.md#retrieve-list).

## 製品化された統合の宛先ドキュメント PR（プルリクエスト）

独立系ソフトウェアベンダー (ISV) またはシステムインテグレータ (SI) の場合、 [製品化統合](./overview.md#productized-custom-integrations)、 [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md) をクリックして、目的の宛先に関する製品ドキュメントページを作成します。 送信プロセスの一環として、宛先ドキュメントのプルリクエスト (PR) を提供します。

既存の統合パートナーの PR の例を次に示します。
* [Yahoo 宛先ドキュメント PR](https://github.com/AdobeDocs/experience-platform.en/pull/110);
* [飛行船の宛先ドキュメント PR](https://github.com/AdobeDocs/experience-platform.en/pull/54).

## 宛先のロゴ

宛先カタログには、各宛先カードのロゴが含まれます。 送信メールに、宛先のロゴを含む画像を含めます。

画像の要件は次のとおりです。
* **形式**: `SVG`
* **サイズ**:2 MB 未満

## サンプルメールをダウンロード

[ダウンロード](./assets/sample-email-submit-destination.rtf) サンプル電子メールと、Adobeに提供する必要のあるすべての情報。