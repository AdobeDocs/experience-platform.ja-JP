---
keywords: OCR；テキストの有無；光学式文字認識
solution: Intelligent Services
title: テキストの有無と光学式文字認識
topic-legacy: Developer guide
description: Content and Commerce AI API では、テキストの有無/光学式文字認識 (OCR) サービスで、特定の画像にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。
exl-id: 85b976a7-0229-43e9-b166-cdbd213b867f
source-git-commit: 16120a10f8a6e3fd7d2143e9f52a822c59a4c935
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 4%

---

# テキストの有無と光学式文字認識

>[!NOTE]
>
>Content and Commerce AI はベータ版です。 このドキュメントは変更される場合があります。

テキストの有無/光学式文字認識 (OCR) サービスは、画像を指定すると、画像内にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。

このドキュメントの例では、次の画像が使用されています。

![テスト画像](../images/shef.jpeg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力画像に基づいて、テキストが存在するかどうかを確認します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを決定するか [!DNL Sensei Content Framework] が使用されます。 適切な `analyzer_id` リクエストを送信する前に Content and Commerce AI ベータチームに連絡し、 `analyzer_id` を設定します。

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
| `analyzer_id` | この [!DNL Sensei] リクエストがデプロイされるサービス ID。 この ID によって [!DNL Sensei Content Frameworks] が使用されます。 カスタムサービスの場合は、Content and Commerce AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | 渡された 1 つの画像を表す配列内の各オブジェクトを持つ JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメータは、 `data` 配列。 この表で説明する残りのプロパティは、内から上書きできます。 `data`. | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトはです。 `inline`. | × |
| `encoding` | 入力画像のファイル形式。 現在、JPEGと PNG 画像のみを処理できます。 このプロパティのデフォルトはです。 `jpeg`. | × |
| `threshold` | 結果を返す必要があるスコアのしきい値 (0 ～ 1)。 値を使用 `0` すべての結果を返します。 このプロパティのデフォルトはです。 `0`. | × |
| `top-N` | 返される結果の数（負の整数は指定できません）。 値を使用 `0` すべての結果を返します。 を `threshold`返される結果の数は、どちらの制限セットの小さい方です。 このプロパティのデフォルトはです。 `0`. | × |
| `custom` | 渡すカスタムパラメーター。 このプロパティを機能させるには、有効な JSON オブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | コンテンツは、生の画像 (「inline」content-type) にすることができます。 <br> コンテンツが S3 上のファイル (「s3-bucket」 content-type) の場合、署名済み URL を渡します。 | ○ |

**応答**

正常な応答は、 `feature_value` 配列。 テキストが読み取られ、左から右に上から下に戻されます。 つまり、「I loveAdobe」が検出された場合、ペイロードは別々のオブジェクトで「I」、「love」および「Adobe」を返します。 オブジェクトでは、 `feature_name` という単語と `feature_value` これには、そのテキストの信頼性指標が含まれます。

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
