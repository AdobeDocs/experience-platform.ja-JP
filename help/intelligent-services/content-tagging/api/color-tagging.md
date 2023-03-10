---
keywords: Experience Platform；はじめに； content ai; commerce ai；コンテンツタグ付け；カラータグ付け；色抽出；
solution: Experience Platform
title: コンテンツタグ付け API のカラータグ付け
description: カラータグ付けサービスは、画像を指定すると、ピクセルカラーのヒストグラムを計算し、主要な色でグループに並べ替えることができます。
exl-id: 6b3b6314-cb67-404f-888c-4832d041f5ed
source-git-commit: feebf4c20d20afcdcfe4523e0b61bff5b999084c
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 5%

---

# カラータグ付け

カラータグ付けサービスは、画像を指定すると、ピクセルカラーのヒストグラムを計算し、主要な色でグループに並べ替えることができます。 イメージピクセル内の色は、カラースペクトルを表す 40 の主要な色にグループ化されます。 次に、これらの 40 色の中から色値のヒストグラムを計算します。 サービスには次の 2 つのバリアントがあります。

**カラータグ付け（全画像）**

この方法では、画像全体のカラーヒストグラムを抽出します。

**カラータグ付け（マスク付き）**

この方法では、ディープラーニングベースのフォアグラウンド抽出を使用して、フォアグラウンドのオブジェクトを識別します。 このモデルは、e コマース画像のカタログに基づいてトレーニングされます。 前景オブジェクトが抽出されると、前述のように、主要な色に対してヒストグラムが計算されます。

このドキュメントの例では、次の画像を使用しています。

![テスト画像](../images/QQAsset1.jpg)

**API 形式**

```http
POST /services/v2/predict
```

**リクエスト**

次のリクエスト例では、色のタグ付けに full-image メソッドを使用しています。

次のリクエストは、ペイロードで指定された入力パラメーターに基づいて画像から色を抽出します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v2/predict \
-H 'Prefer: respond-async, wait=59' \
-H "x-api-key: $API_KEY" \
-H "content-type: multipart/form-data" \
-H "authorization: Bearer $API_TOKEN" \
-F 'contentAnalyzerRequests={
  "sensei:name": "Feature:cintel-image-classifier:Service-60887e328ded447d86e01122a4f19c58",
  "sensei:invocation_mode": "synchronous",
  "sensei:invocation_batch": false,
  "sensei:engines": [
    {
      "sensei:execution_info": {
        "sensei:engine": "Feature:cintel-image-classifier:Service-60887e328ded447d86e01122a4f19c58"
      },
      "sensei:inputs": {
        "documents": [{
            "sensei:multipart_field_name": "infile_1",
            "dc:format": "image/jpg"
          }]
      },
      "sensei:params": {
        "application-id": "1234",
        "enable_mask": 0
      },
      "sensei:outputs":{
        "result" : {
          "sensei:multipart_field_name" : "result",
          "dc:format": "application/json"
        }
      }
    }
  ]
}' \
-F 'infile_1=@1431RDMJANELLERAWJACKE_2.jpg'
```

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `documents` | リスト内の各項目が 1 つのドキュメントを表す JSON 要素のリスト。 | ○ |
| `top_n` | 返される結果の数（負の整数は指定できません）。 値を使用 `0` すべての結果を返します。 を `threshold`返される結果の数は、どちらの制限セットの小さい方です。 このプロパティのデフォルトはです。 `0`. | いいえ |
| `min_coverage` | 結果を返す必要があるカバレッジのしきい値。 すべての結果を返すには、パラメーターを除外します。 | いいえ |
| `resize_image` | 入力画像のサイズを変更するかどうかを示します。 デフォルトでは、画像はカラータグ付けが実行される前に 320*320 ピクセルにサイズ変更されます。 デバッグの目的で、フルイメージでもコードを実行できるようにします。それには、これを False に設定します。 | いいえ |
| `enable_mask` | マスク内のカラータグを有効または無効にします。 | いいえ |

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| -----| --------- | -------- | ------- | ------ | ----------- |
| `repo:path` | 文字列 | - | - | - | キーフレーズの抽出元となるドキュメントの署名済み URL。 |
| `sensei:repoType` | 文字列 | - | - | HTTPS | 画像が保存されるリポジトリのタイプ。 |
| `sensei:multipart_field_name` | 文字列 | - | - | - | 画像ファイルを、署名済みの URL を使用する代わりに、マルチパート引数として渡す場合に使用します。 |
| `dc:format` | 文字列 | ○ | - | &quot;image/jpg&quot;, <br> &quot;image/jpeg&quot;, <br>&quot;image/png&quot;, <br>&quot;image/tiff&quot; | 画像のエンコーディングは、処理前に、許可されている入力エンコーディングタイプと照合されます。 |

**応答**

正常な応答は、抽出された色の詳細を返します。 各色は、 `feature_value` キー。次の情報が含まれます。

- 色名
- この色が画像に対して表示される割合
- 色のRGB値

以下の最初の例のオブジェクトでは、 `feature_value` / `Mud_Green,0.069,102,72,95` は、見つかった色が泥緑、泥緑は画像の 6.9%で見つかり、RGB値が 102,72,95 であることを意味します。

```json
{
  "status": 200,
  "content_id": "test_image.jpg",
  "cas_responses": [
    {
{
  "statuses": [
    {
      "sensei:engine": "Feature:cintel-image-classifier:Service-60887e328ded447d86e01122a4f19c58",
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
  "request_id": "hsxycVq5Q9KbZ7MWrt6NXcSNWbonSLf3"
}

[
  {
    "request_element_id": "0",
    "colors": {
      "Mud_Green": {
        "coverage": 0.0694,
        "rgb": {
          "red": 102,
          "blue": 72,
          "green": 95
        }
      },
      "Dark_Brown": {
        "coverage": 0.1226,
        "rgb": {
          "red": 113,
          "blue": 77,
          "green": 84
        }
      },
      "Pink": {
        "coverage": 0.0731,
        "rgb": {
          "red": 234,
          "blue": 201,
          "green": 209
        }
      },
      "Dark_Gray": {
        "coverage": 0.1533,
        "rgb": {
          "red": 63,
          "blue": 58,
          "green": 59
        }
      },
      "Olive": {
        "coverage": 0.492,
        "rgb": {
          "red": 177,
          "blue": 126,
          "green": 170
        }
      },
      "Brown": {
        "coverage": 0.0896,
        "rgb": {
          "red": 141,
          "blue": 85,
          "green": 105
        }
      }
    }
  }
]
}
```
