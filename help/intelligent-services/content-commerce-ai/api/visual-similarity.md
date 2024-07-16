---
keywords: 視覚的類似性；視覚的類似性；cai api
solution: Experience Platform
title: コンテンツとCommerce AI API の視覚的類似性
description: 画像が指定されると、視覚的類似性サービスは、カタログから視覚的に類似した画像を自動的に見つけます。
exl-id: fe31d9be-ee42-44fa-b83f-3b8a718cb4e3
source-git-commit: b124ed97da8bde2a7fc4f10d350c81a47e096f29
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 3%

---

# 視覚的類似性

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。 ドキュメントは変更される場合があります。

画像が指定されると、視覚的類似性サービスは、カタログから視覚的に類似した画像を自動的に見つけます。

このドキュメントで示すリクエストの例では、次の画像を使用しました。

![ テスト画像 ](../images/Query_Image.jpeg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで指定された入力パラメーターに基づいて、カタログから視覚的に類似した画像を取得します。 表示される入力パラメーターについて詳しくは、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>使用 `analyzer_id` る [!DNL Sensei Content Framework] を指定します。 リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 このサービスの `analyzer_id` ールを受け取るには、コンテンツおよびCommerce AI ベータ版チームにお問い合わせください。

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
| `analyzer_id` | リクエストがデプロイされている [!DNL Sensei] サービス ID。 この ID によって、使用される [!DNL Sensei Content Frameworks] が決まります。 カスタムサービスについては、コンテンツおよびCommerce AI チームに連絡して、カスタム ID の設定を依頼してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | 画像を表す配列内の各オブジェクトを含む JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメーターは、`data` 配列の外部で指定されたグローバルパラメーターよりも優先されます。 この表で説明している残りのプロパティは、`data` 内から上書きできます。 | ○ |
| `content-id` | 応答で返されるデータ要素の一意の ID。 これが渡されない場合、自動生成された ID が割り当てられます。 | × |
| `content` | 視覚的類似性サービスによって分析されるコンテンツ。 画像がリクエスト本文に含まれている場合、curl コマンドで `-F file=@<filename>` を使用して画像を渡し、このパラメーターは空の文字列のままにします。 <br> 画像が S3 上のファイルの場合、署名済み URL を渡します。 コンテンツがリクエスト本文の一部である場合、データ要素のリストにはオブジェクトを 1 つだけ含める必要があります。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |
| `content-type` | 入力がリクエスト本文の一部であるか、S3 バケットの署名済み URL であるかを示すために使用されます。 このプロパティのデフォルトは `inline` です。 | × |
| `encoding` | 入力画像のファイル形式。 現在、処理できるのはJPEG画像と PNG 画像のみです。 このプロパティのデフォルトは `jpeg` です。 | × |
| `threshold` | 結果を返す必要があるしきい値のスコア（0 ～ 1）。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は不可）。 値 `0` を使用して、すべての結果を返します。 `threshold` と一緒に使用すると、返される結果の数は、どちらの制限セットよりも小さくなります。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 | × |
| `historic-metadata` | メタデータを渡すことができる配列。 | × |

**応答**

応答が成功すると、カタログに見つかった、視覚的に類似した各画像の `feature_value` と `feature_name` を含む `response` 配列が返されます。

以下に示す応答の例では、視覚的に類似した次の画像が返されました。

![ 類似画像 ](../images/results.jpg)

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
