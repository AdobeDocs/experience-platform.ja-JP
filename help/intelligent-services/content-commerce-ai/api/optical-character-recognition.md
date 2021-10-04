---
keywords: OCR；テキストの有無；光学式文字認識
solution: Experience Platform, Intelligent Services
title: テキストの有無と光学式文字認識
topic-legacy: Developer guide
description: コンテンツおよびコマース AI API では、テキストの有無/光学式文字認識 (OCR) サービスは、特定の画像にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。
exl-id: 85b976a7-0229-43e9-b166-cdbd213b867f
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 4%

---

# テキストの有無と光学式文字認識

>[!NOTE]
>
>コンテンツおよびコマース AI はベータ版です。 このドキュメントは変更される場合があります。

テキストの有無/光学式文字認識 (OCR) サービスは、画像を指定すると、画像にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。

このドキュメントの例では、次の画像が使用されています。

![テスト画像](../images/shef.jpeg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力画像に基づいて、テキストが存在するかどうかを確認します。 表示される入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを使用す [!DNL Sensei Content Framework] るかを決定します。リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 このサービスの `analyzer_id` を受け取るには、コンテンツおよびコマース AI ベータチームにお問い合わせください。

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
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービス ID。 この ID は、どの [!DNL Sensei Content Frameworks] が使用されるかを決定します。 カスタムサービスの場合は、コンテンツ AI チームとコマース AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | 渡された 1 つの画像を表す配列内の各オブジェクトを持つ JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data` 配列の外部で指定されたグローバルパラメータより優先されます。 この表で説明する残りのプロパティは、`data` 内で上書きできます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトは `inline` です。 | × |
| `encoding` | 入力画像のファイル形式。 現在処理できるのは、JPEG 画像と PNG 画像のみです。 このプロパティのデフォルトは `jpeg` です。 | × |
| `threshold` | スコアのしきい値 (0 ～ 1)。この値を超えると、結果が返されます。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は使用できません）。 値 `0` を使用して、すべての結果を返します。 `threshold` と組み合わせて使用した場合、返される結果の数は、どちらの制限値セットでも小さい方です。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 このプロパティを機能させるには、有効な JSON オブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | コンテンツは、生の画像（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツが S3 上のファイル (「s3-bucket」 content-type) の場合、署名済み URL を渡します。 | ○ |

**応答**

正常な応答は、`feature_value` 配列で検出されたテキストを返します。 テキストが読み取られ、左から右に上から下に戻ります。 つまり、「I loveAdobe」が検出された場合、ペイロードは別々のオブジェクトで「I」、「love」および「Adobe」を返します。 オブジェクトには、という単語を含む `feature_name` と、そのテキストの信頼性指標を含む `feature_value` が表示されます。

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
