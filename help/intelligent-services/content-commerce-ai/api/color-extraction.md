---
keywords: Experience Platform；はじめに； content ai; commerce ai; content and commerce ai；色抽出；色抽出
solution: Experience Platform
title: Content and Commerce AI API での色抽出
topic-legacy: Developer guide
description: カラー抽出サービスは、画像を与えると、ピクセルカラーのヒストグラムを計算し、主要な色でバケットに並べ替えることができます。
exl-id: 6b3b6314-cb67-404f-888c-4832d041f5ed
source-git-commit: eae43834d1cd5931dd752b95023da7ac77668e56
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 3%

---

# カラー抽出

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。 このドキュメントは変更される場合があります。

カラー抽出サービスは、画像を与えると、ピクセルカラーのヒストグラムを計算し、主要な色でバケットに並べ替えることができます。 イメージピクセル内の色は、カラースペクトルを表す 40 の主要な色にグループ化されます。 次に、これらの 40 色の中から色値のヒストグラムを計算します。 サービスには次の 2 つのバリアントがあります。

**カラー抽出（全画像）**

この方法では、画像全体のカラーヒストグラムを抽出します。

**カラー抽出（マスク付き）**

この方法では、ディープラーニングベースのフォアグラウンド抽出を使用して、フォアグラウンドのオブジェクトを識別します。 このモデルは、e コマース画像のカタログに基づいてトレーニングされます。 前景オブジェクトが抽出されると、前述のように、主要な色に対してヒストグラムが計算されます。

このドキュメントの例では、次の画像を使用しています。

![テスト画像](../images/QQAsset1.jpg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエスト例では、色の抽出にフルイメージメソッドを使用しています。

次のリクエストは、ペイロードで指定された入力パラメーターに基づいて画像から色を抽出します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを決定するか [!DNL Sensei Content Framework] が使用されます。 適切な `analyzer_id` リクエストを送信する前に 色抽出サービスの場合、 `analyzer_id` ID:
>`Feature:image-color-histogram:Service-6fe52999293e483b8e4ae9a95f1b81a7`

```SHELL
curl -i -X POST https://sensei.adobe.io/services/v1/predict \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: multipart/form-data' \
  -H 'x-api-key: {API_KEY}' \
  -H 'cache-control: no-cache,no-cache' \
  -F file=@test_image.jpg \
  -F 'contentAnalyzerRequests={
   "enable_diagnostics":"true",
   "requests":[
     {
         "analyzer_id": "Feature:image-color-histogram:Service-6fe52999293e483b8e4ae9a95f1b81a7",
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
            "custom": {"exclude_mask": 1}
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
| `data` | JSON オブジェクトを格納した配列。 配列内の各オブジェクトは画像を表します。 この配列の一部として渡されたパラメータは、 `data` 配列。 この表で説明する残りのプロパティは、内から上書きできます。 `data`. | ○ |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | 色抽出サービスによって分析されるコンテンツ。 画像がリクエスト本文に含まれる場合、 `-F file=@<filename>` curl コマンドで画像を渡し、このパラメーターは空の文字列のままにします。 <br> 画像が S3 上のファイルの場合、署名済み URL を渡します。 コンテンツがリクエスト本文に含まれている場合、データ要素のリストには 1 つのオブジェクトのみを含める必要があります。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトはです。 `inline`. | × |
| `encoding` | 入力画像のファイル形式。 現在、JPEGと PNG 画像のみを処理できます。 このプロパティのデフォルトはです。 `jpeg`. | × |
| `threshold` | 結果を返す必要があるスコアのしきい値 (0 ～ 1)。 値を使用 `0` すべての結果を返します。 このプロパティのデフォルトはです。 `0`. | × |
| `top-N` | 返される結果の数（負の整数は指定できません）。 値を使用 `0` すべての結果を返します。 を `threshold`返される結果の数は、どちらの制限セットの小さい方です。 このプロパティのデフォルトはです。 `0`. | × |
| `custom` | 渡すカスタムパラメーター。 | × |
| `historic-metadata` | メタデータを渡すことができる配列。 | × |

**応答**

正常な応答は、抽出された色の詳細を返します。 各色は、 `feature_value` キー。次の情報が含まれます。

- 色名
- この色が画像に対して表示される割合
- 色のRGB値

以下の最初の例のオブジェクトでは、 `feature_value` / `White,0.59,251,251,243` は、見つかった色が白、白は画像の 59 %に見つかったことを意味し、RGB値は 251,251,243 です。

```json
{
  "status": 200,
  "content_id": "test_image.jpg",
  "cas_responses": [
    {
      "status": 200,
      "analyzer_id": "Feature:image-color-histogram:Service-e952f4acd7c2425199b476a2eb459635",
      "content_id": "test_image.jpg",
      "result": {
        "response_type": "feature",
        "response": [
          {
            "feature_value": [
              {
                "feature_name": "color_name_and_rgb",
                "feature_value": "White,0.59,251,251,243"
              },
              {
                "feature_value": "Orange,0.30,248,169,48",
                "feature_name": "color_name_and_rgb"
              },
              {
                "feature_name": "color_name_and_rgb",
                "feature_value": "Mustard,0.08,251,199,77"
              },
              {
                "feature_name": "color_name_and_rgb",
                "feature_value": "Gold,0.02,250,191,55"
              }
            ],
            "feature_name": "color"
          }
        ]
      }
    }
  ],
  "error": []
}
```

| プロパティ | 説明 |
| --- | --- |
| `content_id` | イメージリクエストでアップロードされたPOSTの名前。 |
| `feature_value` | オブジェクトに同じプロパティ名のキーが含まれる配列。 これらのキーには、色名を表す文字列が含まれます。この色は、 `content_id`、および色のRGB値。 |
