---
keywords: OCR；テキストの有無；光学式文字認識
solution: Experience Platform
title: テキストの有無と光学式文字認識
description: コンテンツタグ付け API では、テキストの有無/光学式文字認識 (OCR) サービスで、特定の画像にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。
exl-id: 85b976a7-0229-43e9-b166-cdbd213b867f
source-git-commit: b124ed97da8bde2a7fc4f10d350c81a47e096f29
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 4%

---

# テキストの有無と光学式文字認識

画像を指定すると、テキストの有無/光学式文字認識 (OCR) サービスは、画像内にテキストが存在するかどうかを示すことができます。 テキストが存在する場合、OCR はテキストを返すことができます。

このドキュメントの例では、次の画像が使用されています。

![サンプル画像](../images/sample_image.png)

**API 形式**

```http
POST /services/v2/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力画像に基づいて、テキストが存在するかどうかを確認します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

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

正常な応答は、 `tags` リストに表示されます。 特定の画像にテキストがない場合、 `is_text_present` が 0 かつ `tags` は空のリストです。

[result0, result1, ...]:各入力ドキュメントの応答のリスト。 それぞれの結果は、次のキーを持つディクトです。

1. request_element_id:この応答の入力ファイルに対応するインデックス。リクエストのドキュメントリストの最初の画像に対しては 0、次の画像に対しては 1 など。
2. タグ：辞書のリストでは、各辞書には次の 2 つのキーがあります。画像から認識される単語であるテキストと関連度。抽出されたテキストのバウンディングボックスの領域の、全画像に対する比率として計算されます。 0.01 を指定すると、画像の少なくとも 1%を占めるテキストに変換されます。
3. is_text_present:0 または 1 は、画像内にテキストが存在するかどうかに応じて異なります。 タグが 0 の場合、リストは空になります。

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
          "relevance": 0.05604639115920341
        }
      ],
      "request_element_id": 0
    }
  ]
}
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力画像に基づいて、テキストが存在するかどうかを確認します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

URL での実行：

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
| `documents` | リスト内の各項目が 1 つの画像を表す JSON 要素のリスト。 このリストの一部として渡されたパラメーターは、対応するリスト要素に対して、リストの外部で指定されたグローバルパラメーターを上書きします。 | ○ |
| `sensei:multipart_field_name` | 入力ファイルのパスを読み込む field_name。 | ○ |
| `repo:path` | 画像アセットの事前署名済み URL。 | ○ |
| `sensei:repoType` | &quot;HTTP&quot; （presigned-url 用） | いいえ |
| `dc:format` | 入力画像のエンコードされた形式。 画像エンコーディングで使用できるのは、jpeg、jpg、png、tiff などの画像形式のみです。 dc:format は、許可された書式と照合されます。 | いいえ |
| `correct_with_dictionary` | 英語の辞書で単語を修正するか。 このオプションをオンにしないと、英語以外の単語が認識される可能性があります。 デフォルトは True です。オン ) 辞書がオンになっている場合、常に英語の単語を取得する必要はありません。 修正を試みますが、ある編集距離内で不可能な場合は、元の単語を返します。 | いいえ |
| `filter_with_dictionary` | 英語の辞書にある単語のみを含むように単語をフィルターするかどうか。 このオプションをオンにすると、返される単語は常に大きな英語に属し、470,000 個の単語で構成されます。 | いいえ |
| `min_probability` | 認識された単語の最小確率はどれくらいですか？ min_probability よりも高い確率を持つ、画像から抽出された単語のみがサービスによって返されます。 デフォルト値は 0.2 に設定されています。 | いいえ |
| `min_relevance` | 認識された単語に対する最小の関連性は何ですか？ サービスは、画像から抽出され、min_relevance よりも関連度が高い単語のみを返します。 デフォルト値は 0.01 に設定されています。関連度は、抽出されたテキストのバウンディングボックスの領域の割合（全画像と比較した場合）として計算されます。 0.01 を指定すると、画像の少なくとも 1%を占めるテキストに変換されます。 | いいえ |

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| -----| --------- | -------- | ------- | ------ | ----------- |
| `repo:path` | 文字列 | - | - | - | テキストの抽出元となる画像の署名済み URL。 |
| `sensei:repoType` | 文字列 | - | - | HTTPS | 画像が保存されるリポジトリのタイプ。 |
| `sensei:multipart_field_name` | 文字列 | - | - | - | 画像を、署名済みの URL を使用する代わりにマルチパート引数として渡す場合に使用します。 |
| `dc:format` | 文字列 | ○ | - | &quot;image/jpg&quot;, <br>&quot;image/jpeg&quot;, <br>&quot;image/png&quot;, <br>&quot;image/tiff&quot; | 画像のエンコーディングは、処理前に、許可されている入力エンコーディングタイプと照合されます。 |