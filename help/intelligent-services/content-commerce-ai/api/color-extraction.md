---
keywords: Experience Platform；はじめに；コンテンツai；コマースai；コンテンツとコマースai；カラー抽出；カラー抽出
solution: Experience Platform, Intelligent Services
title: Content and Commerce AI APIのカラー抽出
topic-legacy: Developer guide
description: カラー抽出サービスは、画像を与えられると、ピクセル色のヒストグラムを計算して、主色でグループに分けることができます。
exl-id: 6b3b6314-cb67-404f-888c-4832d041f5ed
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 2%

---

# カラー抽出

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。このドキュメントは変更されることがあります。

カラー抽出サービスは、画像を与えられると、ピクセル色のヒストグラムを計算して、優良色別にグループに分けることができる。 画像ピクセルの色は、カラースペクトルを表す40の主要な色に分類されます。 次に、これらの40色の中から色値のヒストグラムを算出する。 サービスには2つのバリアントがあります。

**カラー抽出（フルイメージ）**

この方法は、画像全体のカラーヒストグラムを抽出する。

**カラー抽出（マスク付き）**

この方法は、深い学習ベースのフォアグラウンド抽出器を使用して、フォアグラウンドのオブジェクトを識別する。 このモデルは、eコマース画像のカタログに基づいてトレーニングを受けています。 前景オブジェクトが抽出されると、前述のように、主色に対してヒストグラムが計算される。

このドキュメントの例では、次の画像が使用されています。

![テスト画像](../images/QQAsset1.jpg)

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエスト例では、カラー抽出にフルメソッドを使用しています。

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて画像から色を抽出します。 以下に示す入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。リクエストを行う前に、適切な`analyzer_id`があることを確認してください。 カラー抽出サービスの場合、`analyzer_id` IDは：
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
| `analyzer_id` | リクエストの展開先の[!DNL Sensei]サービスID。 このIDは、[!DNL Sensei Content Frameworks]のうちどれを使用するかを決定します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | JSONオブジェクトを含む配列。 配列内の各オブジェクトは、イメージを表します。 この配列の一部として渡されたパラメータは、`data`配列の外部で指定されたグローバルパラメータよりも優先されます。 この表で説明する他のプロパティは、`data`内で上書きできます。 | ○ |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | カラー抽出サービスによって分析されるコンテンツ。 イメージが要求本文に含まれるイベントでは、curlコマンドで`-F file=@<filename>`を使用してイメージを渡し、このパラメーターは空の文字列のままにします。 <br> 画像がS3上のファイルである場合は、署名済みURLを渡します。コンテンツがリクエスト本文の一部である場合、データ要素のリストには1つのオブジェクトしか含めないでください。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトは`inline`です。 | × |
| `encoding` | 入力画像のファイル形式。 現在、処理できるのはJPEGおよびPNG画像のみです。 このプロパティのデフォルトは`jpeg`です。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すには、値`0`を使用します。 このプロパティのデフォルトは`0`です。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すには、値`0`を使用します。 `threshold`と組み合わせて使用した場合、返される結果の数は、どちらの制限セットの中でも小さい方です。 このプロパティのデフォルトは`0`です。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 | × |
| `historic-metadata` | メタデータを渡すことができる配列。 | × |

**応答**

正常に応答すると、抽出された色の詳細が返されます。 各色は`feature_value`キーで表され、次の情報が含まれます。

- 色名
- この色が画像に対して表示される割合
- カラーのRGB値

下の最初の例のオブジェクトでは、`White,0.59,251,251,243`の`feature_value`は、見つかった色が白で、画像の59 %に白が見つかり、RGB値が251,251,243であることを意味します。

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
| `content_id` | POST要求でアップロードされた画像の名前。 |
| `feature_value` | オブジェクトに同じプロパティ名を持つキーが含まれている配列。 これらのキーには、カラー名を表す文字列、`content_id`に送信される画像に対するこのカラーの表示割合、およびカラーのRGB値が含まれます。 |
