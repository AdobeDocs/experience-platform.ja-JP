---
keywords: Experience Platform；はじめに；コンテンツ ai；コマース ai；コンテンツとコマース ai；色抽出；色抽出
solution: Experience Platform, Intelligent Services
title: コンテンツおよびコマース AI API の色抽出
topic-legacy: Developer guide
description: カラー抽出サービスは、画像を与えると、ピクセルカラーのヒストグラムを計算し、主要な色でバケットに並べ替えることができます。
exl-id: 6b3b6314-cb67-404f-888c-4832d041f5ed
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 3%

---

# カラー抽出

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。このドキュメントは変更される場合があります。

カラー抽出サービスは、画像を与えると、ピクセルカラーのヒストグラムを計算し、主要な色でバケットに並べ替えることができます。 画像ピクセルの色は、カラースペクトルを表す 40 の主色に分類されます。 次に、これらの 40 色の中から色値のヒストグラムを計算する。 サービスには次の 2 つのバリエーションがあります。

**カラー抽出（全画像）**

この方法は、画像全体のカラーヒストグラムを抽出します。

**カラー抽出（マスク付き）**

この方法では、ディープラーニングベースのフォアグラウンド抽出を使用して、フォアグラウンドのオブジェクトを識別します。 このモデルは、e コマース画像のカタログに基づいてトレーニングされます。 前景オブジェクトを抽出した後、前述のように、主要な色に対してヒストグラムを計算します。

このドキュメントの例では、次の画像を使用しています。

![テスト画像](../images/QQAsset1.jpg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエスト例では、色抽出にフルイメージメソッドを使用しています。

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて画像から色を抽出します。 表示される入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを使用す [!DNL Sensei Content Framework] るかを決定します。リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 色抽出サービスの場合、 `analyzer_id` ID は次のとおりです。
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
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービス ID。 この ID は、どの [!DNL Sensei Content Frameworks] が使用されるかを決定します。 カスタムサービスの場合は、コンテンツ AI チームとコマース AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | JSON オブジェクトを含む配列。 配列内の各オブジェクトは画像を表します。 この配列の一部として渡されたパラメータは、`data` 配列の外部で指定されたグローバルパラメータより優先されます。 この表で説明する残りのプロパティは、`data` 内で上書きできます。 | ○ |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | 色抽出サービスによって分析されるコンテンツ。 画像が要求本文の一部である場合、 curl コマンドで `-F file=@<filename>` を使用して画像を渡し、このパラメーターは空の文字列のままにします。 <br> 画像が S3 上のファイルの場合、署名済み URL を渡します。コンテンツがリクエスト本文の一部である場合、データ要素のリストには 1 つのオブジェクトのみ含める必要があります。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトは `inline` です。 | × |
| `encoding` | 入力画像のファイル形式。 現在処理できるのは、JPEG 画像と PNG 画像のみです。 このプロパティのデフォルトは `jpeg` です。 | × |
| `threshold` | スコアのしきい値 (0 ～ 1)。この値を超えると、結果が返されます。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は使用できません）。 値 `0` を使用して、すべての結果を返します。 `threshold` と組み合わせて使用した場合、返される結果の数は、どちらの制限値セットでも小さい方です。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 | × |
| `historic-metadata` | メタデータを渡すことができる配列。 | × |

**応答**

正常な応答は、抽出された色の詳細を返します。 各色は `feature_value` キーで表され、次の情報が含まれます。

- 色名
- この色が画像に対して表示される割合
- カラーの RGB 値

以下の最初の例のオブジェクトでは、 `White,0.59,251,251,243` の `feature_value` は、見つかった色が白で、白は画像の 59%で、RGB 値は 251,251,243 です。

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
| `feature_value` | 同じプロパティ名を持つキーを含むオブジェクトを持つ配列。 これらのキーには、色名を表す文字列、`content_id` で送信された画像に対するこの色の表示割合、色の RGB 値が含まれます。 |
