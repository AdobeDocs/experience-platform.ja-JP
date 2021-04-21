---
keywords: 視覚的類似性；視覚的類似性；ccai api
solution: Experience Platform, Intelligent Services
title: コンテンツとコマースのAI APIの視覚の類似性
topic-legacy: Developer guide
description: 視覚類似性サービスは、画像を指定すると、カタログから視覚的に類似した画像を自動的に見つけ出します。
exl-id: fe31d9be-ee42-44fa-b83f-3b8a718cb4e3
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 3%

---

# 視覚的類似性

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。このドキュメントは変更されることがあります。

視覚類似性サービスは、画像を指定すると、カタログから視覚的に類似した画像を自動的に見つけ出します。

このドキュメントのリクエスト例では、次の画像が使用されています。

![テスト画像](../images/Query_Image.jpeg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて、視覚的に類似した画像をカタログから取得します。 以下に示す入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。リクエストを行う前に、適切な`analyzer_id`があることを確認してください。 このサービスの`analyzer_id`を受け取るには、Content and Commerce AIベータチームにお問い合わせください。

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
| `analyzer_id` | リクエストの展開先の[!DNL Sensei]サービスID。 このIDは、[!DNL Sensei Content Frameworks]のうちどれを使用するかを決定します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | 画像を表す配列内の各オブジェクトを持つJSONオブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data`配列の外部で指定されたグローバルパラメータよりも優先されます。 この表で説明する他のプロパティは、`data`内で上書きできます。 | ○ |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | 視覚類似性サービスによって分析されるコンテンツ。 イメージが要求本文に含まれるイベントでは、curlコマンドで`-F file=@<filename>`を使用してイメージを渡し、このパラメーターは空の文字列のままにします。 <br> 画像がS3上のファイルである場合は、署名済みURLを渡します。コンテンツがリクエスト本文の一部である場合、データ要素のリストには1つのオブジェクトしか含めないでください。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトは`inline`です。 | × |
| `encoding` | 入力画像のファイル形式。 現在、処理できるのはJPEGおよびPNG画像のみです。 このプロパティのデフォルトは`jpeg`です。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すには、値`0`を使用します。 このプロパティのデフォルトは`0`です。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すには、値`0`を使用します。 `threshold`と組み合わせて使用した場合、返される結果の数は、どちらの制限セットの中でも小さい方です。 このプロパティのデフォルトは`0`です。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 | × |
| `historic-metadata` | メタデータを渡すことができる配列。 | × |

**応答**

正常に応答すると、カタログ内の視覚的に類似した各画像の`feature_value`と`feature_name`を含む`response`配列が返されます。

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
