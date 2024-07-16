---
keywords: OCR；テキストの存在；光学式文字認識
solution: Experience Platform
title: テキストの存在と光学式文字認識
description: コンテンツのタグ付け API で、テキストの存在/光学式文字認識（OCR）サービスを使用すると、特定の画像にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。
exl-id: 85b976a7-0229-43e9-b166-cdbd213b867f
source-git-commit: 82722ddf7ff543361177b555fffea730a7879886
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 4%

---

# テキストの存在と光学式文字認識

テキストの存在/光学式文字認識（OCR）サービスは、画像が指定された場合、その画像にテキストが存在するかどうかを示す場合があります。 テキストが存在する場合、OCR はテキストを返すことができます。

このドキュメントで示すリクエストの例では、次の画像を使用しました。

![ サンプル画像 ](../images/sample_image.png)

**API 形式**

```http
POST /services/v2/predict
```

**リクエスト**

次のリクエストは、ペイロードで指定された入力画像に基づいて、テキストが存在するかどうかを確認します。 表示される入力パラメーターについて詳しくは、ペイロード例の下の表を参照してください。

インライン画像を使用した実行：

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v2/predict \
-H 'Prefer: respond-async, wait=59' \
-H "x-api-key: $API_KEY" \
-H "content-type: multipart/form-data" \
-H "authorization: Bearer $API_TOKEN" \
-F file=@sample_image.png \
-F 'contentAnalyzerRequests={
  "sensei:name": "Feature:cintel-object-detection:Service-b9ace8b348b6433e9e7d82371aa16690",
  "sensei:invocation_mode": "asynchronous",
  "sensei:invocation_batch": false,
  "sensei:engines": [
    {
      "sensei:execution_info": {
        "sensei:engine": "Feature:cintel-object-detection:Service-b9ace8b348b6433e9e7d82371aa16690"
      },
      "sensei:inputs": {
        "documents": [
        {
          "sensei:multipart_field_name": "file",
          "dc:format": "image/jpg"
        }
        ]
      },
      "sensei:params": {
        "correct_with_dictionary": true,
        "min_probability": 0.2,
        "min_relevance": 0.01,
        "filter_with_dictionary": true
      },
      "sensei:outputs":{
        "result" : {
          "sensei:multipart_field_name" : "result",
          "dc:format": "application/json"
        }
      }
    }
  ]
}'
```

**応答**

応答が成功すると、リクエストで渡された各画像の `tags` リストで検出されたテキストが返されます。 特定の画像にテキストがない場合、`is_text_present` は 0、`tags` は空のリストになります。

[result0, result1, ...]：入力ドキュメントごとの応答のリスト。 各結果は、キーを含む dict です。

1. request_element_id：この応答の入力ファイルに対応するインデックス。リクエストのドキュメントリストの最初の画像の場合は 0、次の画像の場合は 1 などとなります。
2. タグ：辞書のリスト。各辞書には 2 つのキーがあります。text （画像から認識された単語）と、relevance （抽出されたテキストのバウンディングボックスの領域の全画像に対する割合として計算される）です。 0.01 を指定すると、画像の 1% 以上を占めるテキストに変換されます。
3. is_text_present：画像にテキストが存在するかどうかに応じて 0 または 1 を指定します。 tags が 0 の場合、リストは空になります。

```json
{
  "contentAnalyzerResponse": {
    "statuses": [
      {
        "sensei:engine": "Feature:cintel-object-detection:Service-b9ace8b348b6433e9e7d82371aa16690",
        "invocations": [
          {
            "sensei:outputs": {
              "result": {
                "sensei:multipart_field_name": "result",
                "dc:format": "application/json"
              }
            },
            "message": null,
            "status": "200"
          }
        ]
      }
    ],
    "request_id": "dttklFR7DPtMtEmjlRSx5BYP5WGg3tTx"
  },
  "result": [
    {
      "is_text_present": 1,
      "tags": [
        {
          "text": "yosemite",
          "relevance": 0.06
        }
      ],
      "request_element_id": 0
    }
  ]
}
```

**リクエスト**

次のリクエストは、ペイロードで指定された入力画像に基づいて、テキストが存在するかどうかを確認します。 表示される入力パラメーターについて詳しくは、ペイロード例の下の表を参照してください。

実行（URL :）

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v2/predict \
-H 'Prefer: respond-async, wait=59' \
-H "x-api-key: $API_KEY" \
-H "content-type: multipart/form-data" \
-H "authorization: Bearer $API_TOKEN" \
-F 'contentAnalyzerRequests={
  "sensei:name": "Feature:cintel-object-detection:Service-b9ace8b348b6433e9e7d82371aa16690",
  "sensei:invocation_mode": "asynchronous",
  "sensei:invocation_batch": false,
  "sensei:engines": [
    {
      "sensei:execution_info": {
        "sensei:engine": "Feature:cintel-object-detection:Service-b9ace8b348b6433e9e7d82371aa16690"
      },
      "sensei:inputs": {
        "documents": [
        {
          "repo:path": <IMG_URL_PATH>,
          "sensei:repoType": "HTTP",
          "dc:format": "image/jpg"
        }
        ]
      },
      "sensei:params": {
        "correct_with_dictionary": true
      },
      "sensei:outputs":{
        "result" : {
          "sensei:multipart_field_name" : "result",
          "dc:format": "application/json"
        }
      }
    }
  ]
}'
```

```json
{
  "contentAnalyzerResponse": {
    "statuses": [
      {
        "sensei:engine": "Feature:cintel-object-detection:Service-b9ace8b348b6433e9e7d82371aa16690",
        "invocations": [
          {
            "sensei:outputs": {
              "result": {
                "sensei:multipart_field_name": "result",
                "dc:format": "application/json"
              }
            },
            "message": null,
            "status": "200"
          }
        ]
      }
    ],
    "request_id": "ZbdhcK0JqS4Wg1wGdlEHGR3JOm530YNn"
  },
  "result": [
    {
      "is_text_present": 0,
      "tags": [],
      "request_element_id": 0
    }
  ]
}
```

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| `documents` | リスト内の各項目が 1 つの画像を表す JSON 要素のリスト。 このリストの一部として渡されたパラメーターは、対応するリスト要素について、リストの外部で指定されたグローバルパラメーターよりも優先されます。 | ○ |
| `sensei:multipart_field_name` | field_name：入力ファイルのパスを読み取ります。 | ○ |
| `repo:path` | 画像アセットへの事前署名済み URL。 | ○ |
| `sensei:repoType` | 「HTTP」（presigned-url の場合）。 | × |
| `dc:format` | 入力画像のエンコードされた形式。 画像エンコーディングには、jpeg、jpg、png、tiff などの画像形式のみを使用できます。 dc:format は、許可されている形式と照合されます。 | × |
| `correct_with_dictionary` | その単語を英辞書で直せばいいかどうか。 このチェック ボックスにチェックマークが付いていない場合、英字以外の単語が認識される可能性があります。 デフォルトは True です。オンになっています）。 辞書がオンになっているときは、常に英語の単語を取得する必要はありません。 修正しようとしますが、特定の編集距離内で不可能な場合は、元の単語を返します。 | × |
| `filter_with_dictionary` | 英語の辞書の単語のみを含むように単語をフィルターするかどうか。 これをオンにすると、返される単語は常に大きい英語（470,000 語で構成）に属します。 | × |
| `min_probability` | 認識された単語の最小確率はどれくらいですか？ 画像から抽出され、min_probability よりも確率が高い単語のみがサービスによって返されます。 デフォルト値は 0.2 に設定されています。 | × |
| `min_relevance` | 認識された単語の最小関連性は何ですか？ 画像から抽出され、min_relevance よりも関連性の高い単語のみがサービスから返されます。 デフォルト値は 0.01 に設定されています。関連性は、抽出されたテキストのバウンディングボックスの領域の割合として、全画像と比較して計算されます。 0.01 を指定すると、画像の 1% 以上を占めるテキストに変換されます。 | × |

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| -----| --------- | -------- | ------- | ------ | ----------- |
| `repo:path` | 文字列 | - | - | - | テキストを抽出する必要がある画像の事前署名済み URL。 |
| `sensei:repoType` | 文字列 | - | - | HTTPS | 画像が保存されているリポジトリのタイプ。 |
| `sensei:multipart_field_name` | 文字列 | - | - | - | 事前に署名された URL を使用する代わりに、画像をマルチパート引数として渡す場合に、これを使用します。 |
| `dc:format` | 文字列 | ○ | - | &quot;image/jpg&quot;, <br>&quot;image/jpeg&quot;, <br>&quot;image/png&quot;, <br>&quot;image/tiff&quot; | 画像のエンコーディングは、処理前に、許可されている入力エンコーディングタイプと照合されます。 |