---
description: このページでは、Destination SDKを使用して作成された、製品化された宛先のレビュー用に送信する必要があるすべての情報を提供します。
title: 送信してレビュー用に生産済みの宛先をDestination SDKで作成
exl-id: eef0d858-ebd9-426e-91a1-5c93903b0eb5
source-git-commit: 50f205a5ddd9ec264d7390911fef45dc595ca6a1
workflow-type: tm+mt
source-wordcount: '683'
ht-degree: 75%

---

# 送信してレビュー用に生産済みの宛先をDestination SDKで作成

## 概要 {#overview}

>[!IMPORTANT]
>
>* ここで説明するプロセスは、製品化された（公開）宛先をパートナーに送信する場合にのみ必要です。 独自の用途でプライベートな宛先を作成する場合は、これらの資料を作成してAdobeで共有する必要はありません。
>
>* Adobeの宛先の公開要求を確認するための標準的な応答時間は、5 営業日です。
>
>* 初回送信後に設定を更新するようAdobeチームに求められた場合は、更新後に別の公開先要求を送信する必要があります。
>
>* 宛先がExperience Platformカタログに公開された後でも、設定を更新する必要がある場合は、新しい宛先公開要求を送信して、設定に更新が反映されるようにする必要があります。


[Experience Platform 宛先カタログ](/help/destinations/catalog/overview.md)に宛先を公開する前に、プラットフォームにデータをアクティベートする際にアドビに宛先とテストに関する特定の情報を提供し、ユーザーが可能な限り最高のエクスペリエンスを享受できるようにする必要があります。

このページには、Adobe Experience Platform Destination SDK を使用して作成した宛先を送信または更新する際に提供する必要があるすべての情報が一覧表示されます。 Adobe Experience Platform で宛先を正常に送信するには、<aepdestsdk@adobe.com> にメールを送信します。これには以下が含まれます。

* 宛先が解決するユースケースの説明。 既存の宛先設定を更新する場合は、これは必須ではありません。
* 宛先への HTTP 呼び出しを実行するため、テスト宛先 API エンドポイントを使用したテスト結果。アドビと共有してください。
   * 宛先エンドポイントへの API 呼び出し。
   * 宛先エンドポイントから受け取った API 応答。
* [destination publishing API](./destination-publish-api.md) を使用して、宛先の公開リクエストを提出したことの証明。 
* ドキュメント PR（プル要求）。 [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md).
* Experience Platform 宛先カタログに宛先カードのロゴとして表示される画像ファイル。

各項目の詳細については、以下の節を参照してください。

## ユースケースの説明

Experience Platform の顧客用に宛先が解決するユースケースを説明します。 説明は、既存のパートナーのユースケースと類似した内容でも構いません。

* [Pinterest](/help/destinations/catalog/advertising/pinterest.md)：顧客リスト、サイトを訪問した人、または Pinterest でコンテンツとインタラクションを既に経験した人からオーディエンスを作成します。
* [Yahoo Data X](/help/destinations/catalog/advertising/datax.md#use-cases)：Verizon Media（VMG）のメールアドレスをキーに特定の視聴者グループをターゲットにしたい広告主が、VMG のほぼリアルタイムの API を使用して新しいセグメントをすばやく作成し、希望する視聴者グループをプッシュする際には、DataX API を使用できます。

## テスト宛先 API を使用した後のテスト結果

[テスト宛先 API](./test-destination.md) エンドポイントを使用して、宛先への HTTP 呼び出しを実行した後のテスト結果を提供します。 これには以下が含まれます。

* テスト API を使用して、宛先のエンドポイントに送信された完全な API リクエスト（ヘッダーと本文）。
* 宛先エンドポイントから受け取った API 応答。

例えば、リクエストと応答は以下のサンプルのようになります。

**リクエスト**

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

## 宛先の公開リクエストを提出したことの証明

宛先を正常にテストした後、[Destination Publishing API](./destination-publish-api.md) を使用してアドビに送信し、レビューと公開を行う必要があります。

宛先の公開リクエストの ID を指定します。 公開リクエスト ID の取得方法について詳しくは、[宛先公開リクエストのリスト](./destination-publish-api.md#retrieve-list)を参照してください。

## 製品化された統合の宛先ドキュメント PR（プルリクエスト）

独立系ソフトウェアベンダー（ISV）またはシステムインテグレーター（SI）の場合、 [製品化統合](./overview.md#productized-custom-integrations)、 [セルフサービスドキュメントプロセス](./docs-framework/documentation-instructions.md) をクリックして、宛先用に製品ドキュメントページを作成します。 送信プロセスの一環として、宛先ドキュメントのプルリクエスト（PR）を提供します。

## 宛先のロゴ

宛先カタログには、各宛先カードのロゴが含まれます。 提出するメールに、宛先のロゴを含む画像を含めます。

画像の要件は次のとおりです。
* **形式**：`SVG`
* **サイズ**：2 MB 未満

## サンプルメールをダウンロード

サンプルメールと、アドビに提供する必要のあるすべての情報を[ダウンロード](./assets/sample-email-submit-destination.rtf)します。
