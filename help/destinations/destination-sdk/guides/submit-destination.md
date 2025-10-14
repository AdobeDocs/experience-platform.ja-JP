---
description: このページでは、Destination SDKを使用して作成した場合のレビュー用に、製品化された宛先を送信するために必要なすべての情報が提供されます。
title: 製品化した宛先をレビュー用に送信
exl-id: eef0d858-ebd9-426e-91a1-5c93903b0eb5
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '1045'
ht-degree: 35%

---

# 製品化した宛先をレビュー用に送信

## 概要 {#overview}

>[!IMPORTANT]
>
>* ここで説明するプロセスは、パートナーが製品化（公開）された宛先を送信する場合にのみ必要です。 自分で使用するためにプライベートな宛先を作成している場合は、これらの資料を作成してAdobeと共有する必要はありません。
>
>* 宛先の公開リクエストを確認するためのAdobeの標準的な応答時間は 5 営業日です。
>
>* 最初の送信後に設定を更新するようAdobe チームから求められた場合は、更新後に別の公開先リクエストを送信する必要があります。
>
>* 宛先がExperience Platform カタログにライブになっている後でも、設定を更新する必要がある場合は、新しい宛先公開リクエストを送信して、更新を設定に反映させる必要があります。
>
>* レビューのタイムラインと必要なアーティファクトは、更新している新しい宛先と既存の宛先で同じです。

[Experience Platform 宛先カタログ](/help/destinations/catalog/overview.md)に宛先を公開する前に、プラットフォームにデータをアクティベートする際にアドビに宛先とテストに関する特定の情報を提供し、ユーザーが可能な限り最高のエクスペリエンスを享受できるようにする必要があります。

このページには、Adobe Experience Platform Destination SDK を使用して作成した宛先を送信または更新する際に提供する必要があるすべての情報が一覧表示されます。 Adobe Experience Platform で宛先を正常に送信するには、<aepdestsdk@adobe.com> にメールを送信します。これには以下が含まれます。

* 宛先が解決するユースケースの説明。 これは、新しい宛先設定を送信する場合にのみ必要です。
* 宛先の送信理由の説明。 これは、既存の宛先設定を更新する場合にのみ必要です。
* 宛先への HTTP 呼び出しを実行するため、テスト宛先 API エンドポイントを使用したテスト結果。宛先エンドポイントへの API 呼び出しと、宛先エンドポイントから受信した API 応答をAdobeと共有してください。
* 宛先に接続してアクティベーション手順を進めるユーザーのユーザーエクスペリエンスを示す画面録画。
* ファイルベースの宛先に関するその他の要件：
   * テスト API を使用して [&#x200B; サンプルプロファイルを使用してファイルベースの宛先をテストする &#x200B;](../testing-api/batch-destinations/file-based-destination-testing-api.md) 後に、リクエストと応答サンプルを共有します。
   * 宛先で生成され、ストレージの場所に書き出されたサンプルファイルを添付します。
   * 書き出したファイルをストレージの場所からシステムに正常に取り込んだことを証明するフォームを送信します。
* [destination publishing API](../publishing-api/create-publishing-request.md) を使用して、宛先の公開リクエストを提出したことの証明。 
* [&#x200B; セルフサービスドキュメントプロセス &#x200B;](../docs-framework/documentation-instructions.md) に記載されている手順に従った、ドキュメント PR （プルリクエスト）。
* Experience Platform 宛先カタログに宛先カードのロゴとして表示される画像ファイル。

各項目の詳細については、以下の節を参照してください。

## ユースケースの説明 {#use-case-description}

Experience Platform の顧客用に宛先が解決するユースケースを説明します。 説明は、既存のパートナーのユースケースと類似した内容でも構いません。

* [Pinterest](/help/destinations/catalog/advertising/pinterest.md)：顧客リスト、サイトを訪問した人、またはPinterest上のコンテンツとインタラクションを既に経験した人からオーディエンスを作成します。
* [Yahoo Data X](/help/destinations/catalog/advertising/datax.md#use-cases):Verizon Media （VMG）のメールアドレスをキーに特定のオーディエンスグループをターゲットにしたい広告主が、VMG のほぼリアルタイムの API を使用して新しいオーディエンスをすばやく作成し、目的のオーディエンスグループをプッシュする際には、DataX API を使用できます。

## 更新の理由 {#reason-for-update}

>[!NOTE]
>
>このセクションは、既存の設定を更新する場合にのみ必要です。

既存の宛先に対して送信によって解決される問題の簡単な説明を入力します。 例えば、ベータ版から一般提供に移行する際に、送信内容によって宛先の名前、説明、ロゴが更新される場合があります。 または、送信時に、宛先設定で検出されたバグが修正される場合があります。

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

ファイルベースの宛先の場合、宛先を正しく設定したことを示す追加のプルーフを指定する必要があります。 次の項目を必ず含めてください。

### API 応答のテスト {#testing-api-response-file-based}

テスト API を使用して [&#x200B; サンプルプロファイルを使用してファイルベースの宛先をテストする &#x200B;](../testing-api/batch-destinations/file-based-destination-testing-api.md) 後に、リクエストと応答サンプルを含めます。

### 書き出したファイルを添付 {#attach-exported-file}

[&#x200B; 送信メール &#x200B;](#download-sample-email) に、設定した宛先によってお使いのストレージの場所に書き出された CSV ファイルを添付します。

### 取り込みが成功した証拠 {#proof-of-successful-ingestion}

最後に、指定したストレージの場所にデータを書き出した後、データが正常にシステムに取り込まれたかどうかを示す何らかの形の証拠を提供する必要があります。 以下のいずれかの項目を指定してください。

* スクリーンショットまたは簡単なスクリーンキャプチャビデオ。ストレージの場所からファイルを手動で取り込み、システムに取り込みます。
* Experience Platformで生成されたファイル名がシステムに正常に取り込まれたことをシステムの UI が確認する、スクリーンショットまたは簡単なスクリーンキャプチャビデオ。
* Adobeがファイル名またはExperience Platformから生成されたデータと関連付けることができる、システムからのログ行。

## 宛先の公開リクエストを提出したことの証明 {#destination-publishing-request-proof}

宛先を正常にテストした後、[Destination Publishing API](../publishing-api/create-publishing-request.md) を使用してアドビに送信し、レビューと公開を行う必要があります。

宛先の公開リクエストの ID を指定します。 公開リクエスト ID の取得方法について詳しくは、[&#x200B; 宛先公開リクエストの取得 &#x200B;](../publishing-api/retrieve-publishing-request.md) 方法を参照してください。

## 製品化された統合の宛先ドキュメント PR（プルリクエスト） {#documentation-pr}

独立系ソフトウェアベンダー（ISV）またはシステムインテグレーター（SI）の場合、[&#x200B; 製品化統合 &#x200B;](../overview.md#productized-custom-integrations)、[&#x200B; セルフサービスドキュメントプロセス &#x200B;](../docs-framework/documentation-instructions.md) を使用して、宛先用に製品ドキュメントページを作成する必要があります。 送信プロセスの一環として、宛先ドキュメントのプルリクエスト（PR）を提供します。

## 宛先のロゴ {#logo}

宛先カタログには、各宛先カードのロゴが含まれます。 提出するメールに、宛先のロゴを含む画像を含めます。

画像の要件は次のとおりです。
* **形式**：`SVG`
* **サイズ**：2 MB 未満

## サンプルメールをダウンロード {#download-sample-email}

サンプルメールと、アドビに提供する必要のあるすべての情報を[ダウンロード](../assets/guides/sample-email-submit-destination.rtf)します。
