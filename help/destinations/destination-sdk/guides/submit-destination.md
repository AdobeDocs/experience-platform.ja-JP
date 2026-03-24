---
description: このページでは、Destination SDKを使用して作成した場合に製品レベルの宛先をレビュー用に送信するために必要なすべての情報を提供します。
title: 製品化された宛先をレビュー用に送信
exl-id: eef0d858-ebd9-426e-91a1-5c93903b0eb5
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 32%

---

# 製品化された宛先をレビュー用に送信

## 概要 {#overview}

>[!IMPORTANT]
>
>* ここに記載されているプロセスは、製品で構成された（パブリックな）宛先を送信するパートナーにのみ必要です。 自分で使用するプライベートの宛先を作成する場合は、これらの素材を作成してAdobeと共有する必要はありません。
>
>* 宛先の公開リクエストを確認するためのAdobeの標準的な応答時間は5営業日です。
>
>* 最初の送信後に設定を更新するようにAdobe チームから求められた場合は、更新を行った後、別の宛先パブリッシュリクエストを送信する必要があります。
>
>* 宛先がExperience Platform カタログに公開された後でも、設定を更新する必要がある場合は、更新を設定に反映するために、新しい宛先の公開リクエストを送信する必要があります。
>
>* レビュータイムラインと必要なアーティファクトは、更新中の新しい宛先と既存の宛先で同じです。

[Experience Platform 宛先カタログ](/help/destinations/catalog/overview.md)に宛先を公開する前に、プラットフォームにデータをアクティベートする際にアドビに宛先とテストに関する特定の情報を提供し、ユーザーが可能な限り最高のエクスペリエンスを享受できるようにする必要があります。

このページには、[!DNL Adobe Experience Platform] Destination SDKを使用して作成した宛先を送信または更新する際に提供する必要があるすべての情報が一覧表示されます。 [!DNL Adobe Experience Platform]の宛先を正常に送信するには、次の内容を含むメールを<aepdestsdk@adobe.com>に送信します。

* 宛先が解決するユースケースの説明。 これは、新しい宛先設定を送信する場合にのみ必要です。
* 宛先送信理由の説明。 これは、既存の宛先設定を更新する場合にのみ必要です。
* 宛先への HTTP 呼び出しを実行するため、テスト宛先 API エンドポイントを使用したテスト結果。宛先エンドポイントに対して行われたAPI呼び出しと、宛先エンドポイントから受信したAPI応答をAdobeに送信してください。
* 宛先に接続し、アクティベーション手順を進めるユーザーのエクスペリエンスを示す画面記録。
* ファイルベースの宛先に関するその他の要件：
   * テスト APIを使用した後、リクエストと応答サンプルを共有して、[ サンプルプロファイルを使用してファイルベースの宛先をテスト ](../testing-api/batch-destinations/file-based-destination-testing-api.md)します。
   * 宛先によって生成され、保存場所に書き出されたサンプルファイルを添付します。
   * 書き出したファイルをストレージの場所からシステムに正常に取り込んだことを証明する何らかの形式のプルーフを送信します。
* [destination publishing API](../publishing-api/create-publishing-request.md) を使用して、宛先の公開リクエストを提出したことの証明。 
* [ セルフサービスのドキュメントプロセス ](../docs-framework/documentation-instructions.md)で説明されている手順に従って、ドキュメント PR （プル要求）を実行します。
* Experience Platform 宛先カタログに宛先カードのロゴとして表示される画像ファイル。

各項目の詳細については、以下の節を参照してください。

## ユースケースの説明 {#use-case-description}

Experience Platform の顧客用に宛先が解決するユースケースを説明します。 説明は、既存のパートナーのユースケースと類似した内容でも構いません。

* [Pinterest](/help/destinations/catalog/advertising/pinterest.md)：お客様リスト、サイトを訪問した人物、またはPinterestでコンテンツを既に操作した人物からオーディエンスを作成します。
* [Yahoo Data X](/help/destinations/catalog/advertising/datax.md#use-cases): Verizon Media （VMG）の電子メールアドレスに基づいて特定のオーディエンスグループをターゲットにする広告主は、DataX APIを使用できます。これにより、新しいオーディエンスをすばやく作成し、VMGのほぼリアルタイム APIを使用して目的のオーディエンスグループをプッシュできます。

## 更新の理由 {#reason-for-update}

>[!NOTE]
>
>このセクションは、既存の設定を更新する場合にのみ必要です。

既存の宛先に対して、送信が解決する問題の簡単な説明を入力します。 例えば、ベータ版から一般公開に移行する際に、送信先の名前、説明、ロゴを更新します。 あるいは、送信先の設定で発見されたバグを提出で修正する場合があります。

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

## ファイルベースの宛先に関する追加要件 {#additional-file-based-destination-requirements}

ファイルベースの宛先の場合は、宛先を正しく設定したことを証明する追加の証明書を指定する必要があります。 次の項目が含まれていることを確認してください。

### API応答のテスト {#testing-api-response-file-based}

テスト APIを使用した後、リクエストと応答サンプルを含めて、[ サンプルプロファイルを使用してファイルベースの宛先をテスト ](../testing-api/batch-destinations/file-based-destination-testing-api.md)します。

### 書き出したファイルを添付 {#attach-exported-file}

[送信メール ](#download-sample-email)で、設定した宛先によってストレージの場所に書き出されたCSV ファイルを添付します。

### 取り込みが成功したことを示す証拠 {#proof-of-successful-ingestion}

最後に、提供したストレージの場所にデータが書き出された後、データが正常にシステムに取り込まれたという何らかの形の証明を提供する必要があります。 以下のいずれかの項目を入力してください。

* スクリーンショットまたは簡単なスクリーンキャプチャビデオ。このビデオでは、ファイルをストレージの場所から手動で取り出し、システムに取り込みます。
* Experience Platformによって生成されたファイル名がシステムに正常に取り込まれたことを確認する、システムのUIが表示されたスクリーンショットまたは簡単なスクリーンキャプチャビデオ。
* Adobeがファイル名またはExperience Platformから生成されたデータのいずれかに関連付けることができるシステムからのログ行。

## 宛先の公開リクエストを提出したことの証明 {#destination-publishing-request-proof}

宛先を正常にテストした後、[Destination Publishing API](../publishing-api/create-publishing-request.md) を使用してアドビに送信し、レビューと公開を行う必要があります。

宛先の公開リクエストの ID を指定します。 パブリッシュ要求IDの取得方法について詳しくは、「[宛先のパブリッシュ要求を取得する方法](../publishing-api/retrieve-publishing-request.md)」を参照してください。

## 製品化された統合の宛先ドキュメント PR（プルリクエスト） {#documentation-pr}

[製品化された統合](../overview.md#productized-custom-integrations)を作成する独立ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）の場合は、[ セルフサービスのドキュメントプロセス ](../docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを作成する必要があります。 送信プロセスの一環として、宛先ドキュメントのプルリクエスト（PR）を提供します。

## 宛先のロゴ {#logo}

宛先カタログには、各宛先カードのロゴが含まれます。 提出するメールに、宛先のロゴを含む画像を含めます。

画像の要件は次のとおりです。

* **形式**：`SVG`
* **サイズ**：2 MB 未満

## サンプルメールをダウンロード {#download-sample-email}

サンプルメールと、アドビに提供する必要のあるすべての情報を[ダウンロード](../assets/guides/sample-email-submit-destination.rtf)します。
