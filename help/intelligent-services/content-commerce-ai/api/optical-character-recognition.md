---
keywords: OCR;text presence;optical character recognition
solution: Experience Platform
title: 光学式文字認識
topic: Developer guide
description: テキストの存在/光学式文字認識(OCR)サービスは、画像を指定した場合、画像内にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCRはテキストを返すことができます
translation-type: tm+mt
source-git-commit: 4d12caf949aeb6619cd27b55855014a61d4e54bb
workflow-type: tm+mt
source-wordcount: '512'
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
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。 リクエストを行う前に、適切な情報があることを確認し `analyzer_id` てください。 本サービスのご利用を受けるには、コンテンツおよびコマースAIベータチームにお問い合わせ `analyzer_id` ください。

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
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービスID。 このIDは、使用するIDを決定 [!DNL Sensei Content Frameworks] します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | 渡された1つの画像を表す配列内の各オブジェクトを持つJSONオブジェクトを含む配列。 この配列の一部として渡されたパラメータは、配列の外部で指定されたグローバルパラメータよりも優先され `data` ます。 次の表に示す残りのプロパティは、内で上書きでき `data`ます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトはで `inline`す。 | × |
| `encoding` | 入力画像のファイル形式。 現在、処理できるのはJPEGおよびPNG画像のみです。 このプロパティのデフォルトはで `jpeg`す。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すに `0` は、この値を使用します。 このプロパティのデフォルトはで `0`す。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すに `0` は、この値を使用します。 と組み合わせて使用した場合、返される結果の数は、どちらの制限セットにも該当しない数になります。 `threshold`このプロパティのデフォルトはで `0`す。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 このプロパティを機能させるには、有効なJSONオブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | コンテンツは、生の画像（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツがS3上のファイル(「s3-bucket」 content-type)の場合、署名済みURLを渡します。 | ○ |

**応答** 

正常に応答すると、 `feature_value` 配列内で検出されたテキストが返されます。 テキストが読み取られ、左から右に上から下に返されます。 つまり、「I loveAdobe」が検出された場合、ペイロードは、別のオブジェクトで「I」、「love」および「Adobe」を返します。 オブジェクトには、その単語を含む `feature_name` と、そのテキストの信頼性指標 `feature_value` を含むという語が与えられます。

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
