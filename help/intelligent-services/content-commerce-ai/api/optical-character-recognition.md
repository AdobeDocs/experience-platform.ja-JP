---
keywords: OCR；テキストの存在；光学式文字認識
solution: Experience Platform, Intelligent Services
title: テキストの存在と光学式文字認識
topic-legacy: Developer guide
description: Content and Commerce AI APIでは、Text Presence / Optical Character Recognition(OCR)サービスは、特定の画像にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCRはテキストを返すことができます。
exl-id: 85b976a7-0229-43e9-b166-cdbd213b867f
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 4%

---

# テキストの存在と光学式文字認識

>[!NOTE]
>
>コンテンツとコマースAIはベータ版です。 このドキュメントは変更されることがあります。

テキストの存在/光学式文字認識(OCR)サービスは、画像を指定した場合、画像内にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCRはテキストを返すことができます。

このドキュメントのリクエスト例では、次の画像が使用されています。

![テスト画像](../images/shef.jpeg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

以下のリクエストは、ペイロードで提供された入力画像に基づいて、テキストが存在するかどうかを確認します。 以下に示す入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。リクエストを行う前に、適切な`analyzer_id`があることを確認してください。 このサービスの`analyzer_id`を受け取るには、Content and Commerce AIベータチームにお問い合わせください。

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v1/predict \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "Content-Type: multipart/form-data" \
  -H "cache-control: no-cache,no-cache" \
  -H "x-api-key: {API_KEY}" \
  -F file=@TestImage.jpg \
  -F 'contentAnalyzerRequests={
    "enable_diagnostics":"true",
    "requests":[{
    "analyzer_id": "Feature:image-text-extractor-ocr:Service-b0675160421e404ca3c7ca60f46a5b29",
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
    }]
  }'
```

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| `analyzer_id` | リクエストの展開先の[!DNL Sensei]サービスID。 このIDは、[!DNL Sensei Content Frameworks]のうちどれを使用するかを決定します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | 渡された1つの画像を表す配列内の各オブジェクトを持つJSONオブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data`配列の外部で指定されたグローバルパラメータよりも優先されます。 この表で説明する他のプロパティは、`data`内で上書きできます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトは`inline`です。 | × |
| `encoding` | 入力画像のファイル形式。 現在、処理できるのはJPEGおよびPNG画像のみです。 このプロパティのデフォルトは`jpeg`です。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すには、値`0`を使用します。 このプロパティのデフォルトは`0`です。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すには、値`0`を使用します。 `threshold`と組み合わせて使用した場合、返される結果の数は、どちらの制限セットの中でも小さい方です。 このプロパティのデフォルトは`0`です。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 このプロパティを機能させるには、有効なJSONオブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | コンテンツは、生の画像（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツがS3上のファイル(「s3-bucket」 content-type)の場合、署名済みURLを渡します。 | ○ |

**応答**

正常に応答すると、`feature_value`配列で検出されたテキストが返されます。 テキストが読み取られ、左から右に上から下に返されます。 つまり、「I loveAdobe」が検出された場合、ペイロードは、別のオブジェクトで「I」、「love」および「Adobe」を返します。 オブジェクトには、単語を含む`feature_name`と、そのテキストの信頼性指標を含む`feature_value`が与えられます。

```json
{
  "status": 200,
  "content_id": "TestImage.jpg",
  "cas_responses": [
    {
      "status": 200,
      "analyzer_id": "Feature:image-text-extractor-ocr:Service-b0675160421e404ca3c7ca60f46a5b29",
      "content_id": "TestImage.jpg",
      "result": {
        "response_type": "feature",
        "response": [
          {
            "feature_value": [
              {
                "feature_value": "yes",
                "feature_name": "has_text"
              },
              {
                "feature_value": "0.977",
                "feature_name": "CHEF"
              },
              {
                "feature_value": "success",
                "feature_name": "text_processing_status"
              }
            ],
            "feature_name": "ocr"
          }
        ]
      }
    }
  ],
  "error": []
}
```
