---
description: このページでは、Destination SDKを使用して作成された、製品化された宛先のレビュー用に送信する必要があるすべての情報を提供します。
title: 送信してレビュー用に生産済みの宛先をDestination SDKで作成
exl-id: eef0d858-ebd9-426e-91a1-5c93903b0eb5
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '1014'
ht-degree: 36%

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

* 宛先が解決するユースケースの説明。 これは、新しい宛先設定を送信する場合にのみ必要です。
* 宛先の送信理由の説明。 これは、既存の宛先設定を更新する場合にのみ必要です。
* 宛先への HTTP 呼び出しを実行するため、テスト宛先 API エンドポイントを使用したテスト結果。宛先エンドポイントに対する API 呼び出しと、Adobeエンドポイントから受け取った API 応答を宛先と共有してください。
* ファイルベースの宛先に関するその他の要件：
   * テスト API を使用した後、リクエストと応答サンプルを [サンプルプロファイルを使用したファイルベースの宛先のテスト](../testing-api/batch-destinations/file-based-destination-testing-api.md).
   * 宛先で生成され、ストレージの場所に書き出されたサンプルファイルを添付します。
   * 書き出されたファイルをストレージの場所からシステムに正常に取り込んだことを示す、何らかの配達確認の形式を送信します。
* [destination publishing API](../publishing-api/create-publishing-request.md) を使用して、宛先の公開リクエストを提出したことの証明。 
* ドキュメント PR（プル要求）。 [セルフサービスドキュメント化プロセス](../docs-framework/documentation-instructions.md).
* Experience Platform 宛先カタログに宛先カードのロゴとして表示される画像ファイル。

各項目の詳細については、以下の節を参照してください。

## ユースケースの説明 {#use-case-description}

Experience Platform の顧客用に宛先が解決するユースケースを説明します。 説明は、既存のパートナーのユースケースと類似した内容でも構いません。

* [Pinterest](/help/destinations/catalog/advertising/pinterest.md):顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成します。
* [Yahoo データ X](/help/destinations/catalog/advertising/datax.md#use-cases):Verizon Media(VMG) の電子メールアドレスをキーにした特定のオーディエンスグループをターゲットにしたい広告主は、VMG のほぼリアルタイム API を使用して、新しいオーディエンスをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## 更新の理由 {#reason-for-update}

>[!NOTE]
>
>このセクションは、既存の設定を更新する場合にのみ必要です。

既存の宛先に対して送信で解決する問題の簡単な説明を提供します。 例えば、ベータ版から一般提供版に移行すると、送信先の名前、説明、ロゴが更新される場合があります。 または、送信によって、宛先設定で見つかったバグが修正される場合があります。

## テスト宛先 API を使用した後のテスト結果 {#testing-api-response}

[テスト宛先 API](../testing-api/streaming-destinations/streaming-destination-testing-overview.md) エンドポイントを使用して、宛先への HTTP 呼び出しを実行した後のテスト結果を提供します。 これには以下が含まれます。

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

## ファイルベースの宛先に関するその他の要件 {#additional-file-based-destination-requirements}

ファイルベースの宛先の場合、宛先が正しく設定されていることを示す追加の配達確認を提供する必要があります。 次の項目を必ず含めてください。

### API 応答のテスト {#testing-api-response-file-based}

テスト API を使用した後に、リクエストと応答のサンプルを [サンプルプロファイルを使用したファイルベースの宛先のテスト](../testing-api/batch-destinations/file-based-destination-testing-api.md).

### 書き出したファイルを添付 {#attach-exported-file}

を [送信メール](#download-sample-email)、設定した宛先でストレージの場所に書き出された CSV ファイルを添付します。

### 取り込み成功の証明 {#proof-of-successful-ingestion}

最後に、指定したストレージの場所にデータが書き出された後、データがシステムに正常に取り込まれたことを示す何らかの形の証明を提供する必要があります。 以下の項目のいずれかを指定してください。

* ストレージの場所から手動でファイルを取り出し、システムに取り込むスクリーンショットまたは短いスクリーンキャプチャビデオ。
* システムの UI で、Experience Platformによって生成されたファイル名がシステムに正常に取り込まれたことを確認するスクリーンショットや短いスクリーンキャプチャビデオ。
* Adobeがファイル名またはExperience Platformから生成されたデータと関連付けることのできる、システムの行をログに記録します。

## 宛先の公開リクエストを提出したことの証明 {#destination-publishing-request-proof}

宛先を正常にテストした後、[Destination Publishing API](../publishing-api/create-publishing-request.md) を使用してアドビに送信し、レビューと公開を行う必要があります。

宛先の公開リクエストの ID を指定します。 公開リクエスト ID の取得方法について詳しくは、 [宛先の公開リクエストを取得](../publishing-api/retrieve-publishing-request.md).

## 製品化された統合の宛先ドキュメント PR（プルリクエスト） {#documentation-pr}

独立系ソフトウェアベンダー (ISV) またはシステムインテグレータ (SI) の場合、 [製品化統合](../overview.md#productized-custom-integrations)を使用する場合、 [セルフサービスドキュメント化プロセス](../docs-framework/documentation-instructions.md) をクリックして、目的の宛先に関する製品ドキュメントページを作成します。 送信プロセスの一環として、宛先ドキュメントのプルリクエスト（PR）を提供します。

## 宛先のロゴ {#logo}

宛先カタログには、各宛先カードのロゴが含まれます。 提出するメールに、宛先のロゴを含む画像を含めます。

画像の要件は次のとおりです。
* **形式**：`SVG`
* **サイズ**：2 MB 未満

## サンプルメールをダウンロード {#download-sample-email}

サンプルメールと、アドビに提供する必要のあるすべての情報を[ダウンロード](../assets/guides/sample-email-submit-destination.rtf)します。
