---
keywords: 視覚的類似性；視覚的類似性；ccai api
solution: Experience Platform
title: Content および Commerce AI API の視覚的類似性
description: 視覚類似性サービスは、画像を指定すると、カタログから視覚的に類似した画像を自動的に見つけます。
exl-id: fe31d9be-ee42-44fa-b83f-3b8a718cb4e3
source-git-commit: b124ed97da8bde2a7fc4f10d350c81a47e096f29
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 3%

---

# 視覚的類似性

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。 このドキュメントは変更される場合があります。

視覚類似性サービスは、画像を指定すると、カタログから視覚的に類似した画像を自動的に見つけます。

このドキュメントの例では、次の画像が使用されています。

![テスト画像](../images/Query_Image.jpeg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで指定された入力パラメーターに基づいて、カタログから視覚的に類似した画像を取得します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを決定するか [!DNL Sensei Content Framework] が使用されます。 適切な `analyzer_id` リクエストを送信する前に Content and Commerce AI ベータチームに連絡し、 `analyzer_id` を設定します。

```SHELL
curl -i -X POST https://sensei.adobe.io/services/v1/predict \
  -H 'Authorization: Bearer $API_TOKEN' \
  -H 'Content-Type: multipart/form-data' \
  -H 'cache-control: no-cache,no-cache' \
  -H 'x-api-key: $API_KEY' \
  -F file=@test_image.jpg \
  -F 'contentAnalyzerRequests={
   "enable_diagnostics":"true",
   "requests":[
     {
         "analyzer_id": "Feature:cintel-deep-product-search:Service-316a8cf750c6440396061c8f73a7a585",
         "parameters": {
          "application-id": "1234", 
          "content-type": "inline", 
          "encoding": "jpeg", 
          "threshold": "0", 
          "top-N": "0", 
          "custom": {}, 
          "data": [{
            "content-id": "0987", 
            "content": "inline-image", 
            "content-type": "inline", 
            "encoding": "jpeg", 
            "threshold": "0", 
            "top-N": "0", 
            "historic-metadata": [], 
            "custom": {}
            }]
          }
      }
    ]
}'
```

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| `analyzer_id` | この [!DNL Sensei] リクエストがデプロイされるサービス ID。 この ID によって [!DNL Sensei Content Frameworks] が使用されます。 カスタムサービスの場合は、Content and Commerce AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | 配列内の各オブジェクトが画像を表す JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメータは、 `data` 配列。 この表で説明する残りのプロパティは、内から上書きできます。 `data`. | ○ |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | いいえ |
| `content` | 視覚類似性サービスによって分析されるコンテンツ。 画像がリクエスト本文に含まれる場合、 `-F file=@<filename>` curl コマンドで画像を渡し、このパラメーターは空の文字列のままにします。 <br> 画像が S3 上のファイルの場合、署名済み URL を渡します。 コンテンツがリクエスト本文に含まれている場合、データ要素のリストには 1 つのオブジェクトのみを含める必要があります。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトはです。 `inline`. | いいえ |
| `encoding` | 入力画像のファイル形式。 現在、JPEGと PNG 画像のみを処理できます。 このプロパティのデフォルトはです。 `jpeg`. | いいえ |
| `threshold` | 結果を返す必要があるスコアのしきい値 (0 ～ 1)。 値を使用 `0` すべての結果を返します。 このプロパティのデフォルトはです。 `0`. | いいえ |
| `top-N` | 返される結果の数（負の整数は指定できません）。 値を使用 `0` すべての結果を返します。 を `threshold`返される結果の数は、どちらの制限セットの小さい方です。 このプロパティのデフォルトはです。 `0`. | いいえ |
| `custom` | 渡すカスタムパラメーター。 | いいえ |
| `historic-metadata` | メタデータを渡すことができる配列。 | いいえ |

**応答**

成功すると、 `response` 配列に含まれる `feature_value` および `feature_name` カタログ内の視覚的に類似した画像ごとに

次の例の応答では、視覚的に類似した画像が返されています。

![類似画像](../images/results.jpg)

```json
{
  "status": 200,
  "content_id": "test_image.jpg",
  "cas_responses": [
    {
      "status": 200,
      "analyzer_id": "Feature:cintel-deep-product-search:Service-316a8cf750c6440396061c8f73a7a585",
      "content_id": "test_image.jpg",
      "result": {
        "response_type": "feature",
        "response": [
          {
            "feature_value": [
              {
                "feature_value": "678",
                "feature_name": "G34WS945.F1"
              },
              {
                "feature_value": "678",
                "feature_name": "1431RDM JANELLE RAW JACKE"
              },
              {
                "feature_value": "657",
                "feature_name": "GF4045877841 CARLA FLR"
              },
              {
                "feature_name": "1707-686-SGU PATCH XYZ",
                "feature_value": "657"
              },
              {
                "feature_name": "5495MJT AJA BLK",
                "feature_value": "646"
              },
              {
                "feature_name": "IDEAL",
                "feature_value": "645"
              },
              {
                "feature_value": "644",
                "feature_name": "HCAJRA439 CALI JEAN"
              },
              {
                "feature_name": "KT279RK-ONL",
                "feature_value": "644"
              },
              {
                "feature_name": "SP190404-ELLIS",
                "feature_value": "642"
              },
              {
                "feature_name": "GF4174848718 KENDALL DIS",
                "feature_value": "640"
              }
            ],
            "feature_name": "visual_similarity"
          }
        ]
      }
    }
  ],
  "error": []
}
```
