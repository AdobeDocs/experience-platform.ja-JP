---
keywords: Experience Platform；はじめに；コンテンツ；コンテンツタグ付け；カラータグ付け；色抽出；
solution: Experience Platform
title: コンテンツタグ付け API のカラータグ付け
description: カラータグ付けサービスは、画像を指定すると、ピクセルカラーのヒストグラムを計算し、主要な色でグループに並べ替えることができます。
exl-id: 6b3b6314-cb67-404f-888c-4832d041f5ed
source-git-commit: fd8891bdc7d528e327d2a72c2427f7bbc6dc8a03
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 6%

---

# カラータグ付け

カラータグ付けサービスは、画像を指定すると、ピクセルカラーのヒストグラムを計算し、主要な色でグループに並べ替えることができます。 イメージピクセル内の色は、カラースペクトルを表す 40 の主要な色にグループ化されます。 次に、これらの 40 色の中から色値のヒストグラムを計算します。 サービスには次の 2 つのバリアントがあります。

**カラータグ付け（全画像）**

この方法では、画像全体のカラーヒストグラムを抽出します。

**カラータグ付け（マスク付き）**

この方法では、ディープラーニングベースのフォアグラウンド抽出を使用して、フォアグラウンドにあるオブジェクトを識別します。 前景オブジェクトが抽出されると、前景領域と背景領域の両方の主要な色に対して、画像全体と共にヒストグラムが計算されます。

**トーン抽出**

上記のバリアントに加えて、次のトーンのヒストグラムを取得するようにサービスを設定できます。

- 画像全体（フル画像バリアントを使用する場合）
- 画像全体、および前景と背景の領域（マスクを使用してバリアントを使用する場合）

このドキュメントの例では、次の画像を使用しています。

![テスト画像](../images/QQAsset1.jpg)

**API 形式**

```http
POST /services/v2/predict
```

**リクエスト — 完全な画像バリアント**

次のリクエスト例では、フルイメージメソッドを使用してカラータグ付けを行い、ペイロードで指定された入力パラメーターに基づいて画像から色を抽出します。 表示される入力パラメーターの詳細については、サンプルのペイロードの下の表を参照してください。

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v2/predict \
-H 'Prefer: respond-async, wait=59' \
-H "x-api-key: $API_KEY" \
-H "content-type: multipart/form-data" \
-H "authorization: Bearer $API_TOKEN" \
-F 'contentAnalyzerRequests={
  "sensei:name": "Feature:autocrop:Service-af865523d46547e2b17fdf9b38e32a72",
  "sensei:invocation_mode": "synchronous",
  "sensei:invocation_batch": false,
  "sensei:engines": [
    {
      "sensei:execution_info": {
        "sensei:engine": "Feature:autocrop:Service-af865523d46547e2b17fdf9b38e32a72"
      },
      "sensei:inputs": {
        "documents": [{
            "sensei:multipart_field_name": "infile_1",
            "dc:format": "image/jpg"
          }]
      },
      "sensei:params": {
        "top_n": 5,
        "min_coverage": 0.005      
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

**応答 — 完全な画像のバリアント**

正常な応答は、抽出された色の詳細を返します。 各色は、 `feature_value` キー。次の情報が含まれます。

- 色名
- この色が画像に対して表示される割合
- 色のRGB値

`"White":{"coverage":0.5834,"rgb":{"red":254,"green":254,"blue":243}}`は、見つかった色が白で、画像の 58.34%に見つかり、平均RGB値が 254、254、243 であることを意味します。

```json
{
    "statuses": [{
        "sensei:engine": "Feature:autocrop:Service-af865523d46547e2b17fdf9b38e32a72",
        "invocations": [{
            "sensei:outputs": {
                "result": {
                    "sensei:multipart_field_name": "result",
                    "dc:format": "application/json"
                }
            },
            "message": null,
            "status": "200"
        }]
    }],
    "request_id": "bfpzaJxKDxtgxpjUj5QDrN1jasjUw2RM"
}  
 
[{
    "overall": {
        "colors": {
            "White": {
                "coverage": 0.5834,
                "rgb": {
                    "red": 254,
                    "green": 254,
                    "blue": 243
                }
            },
            "Orange": {
                "coverage": 0.254,
                "rgb": {
                    "red": 249,
                    "green": 165,
                    "blue": 45
                }
            },
            "Gold": {
                "coverage": 0.0817,
                "rgb": {
                    "red": 253,
                    "green": 188,
                    "blue": 58
                }
            },
            "Mustard": {
                "coverage": 0.0727,
                "rgb": {
                    "red": 253,
                    "green": 207,
                    "blue": 84
                }
            },
            "Cream": {
                "coverage": 0.0082,
                "rgb": {
                    "red": 253,
                    "green": 236,
                    "blue": 174
                }
            }
        }
    }
}]
```

この例では、「全体」の画像領域に色が抽出されています。

**リクエスト — マスクされた画像のバリアント**

次のリクエスト例では、色のタグ付けにマスクメソッドを使用します。 これは、 `enable_mask` パラメータ `true` リクエストに含まれます。

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v2/predict \
-H 'Prefer: respond-async, wait=59' \
-H "x-api-key: $API_KEY" \
-H "content-type: multipart/form-data" \
-H "authorization: Bearer $API_TOKEN" \
-F 'contentAnalyzerRequests={
  "sensei:name": "Feature:autocrop:Service-af865523d46547e2b17fdf9b38e32a72",
  "sensei:invocation_mode": "synchronous",
  "sensei:invocation_batch": false,
  "sensei:engines": [
    {
      "sensei:execution_info": {
        "sensei:engine": "Feature:autocrop:Service-af865523d46547e2b17fdf9b38e32a72"
      },
      "sensei:inputs": {
        "documents": [{
            "sensei:multipart_field_name": "infile_1",
            "dc:format": "image/jpg"
          }]
      },
      "sensei:params": {
        "top_n": 5,
        "min_coverage": 0.005,
        "enable_mask": true,
        "retrieve_tone": true     
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

>[!NOTE]
>
>また、 `retrieve_tone` パラメータも `true` 上記のリクエストの。 これにより、画像の全体領域、前景領域、背景領域の暖かいトーン、中立トーン、冷たいトーンに対して、トーン分布ヒストグラムを取り出すことができます。

**応答 — マスクされた画像のバリアント**

```json
{
    "statuses": [{
        "sensei:engine": "Feature:autocrop:Service-af865523d46547e2b17fdf9b38e32a72",
        "invocations": [{
            "sensei:outputs": {
                "result": {
                    "sensei:multipart_field_name": "result",
                    "dc:format": "application/json"
                }
            },
            "message": null,
            "status": "200"
        }]
    }],
    "request_id": "gpeCyJsrJvOWd94WwZOyPBPrKi2BQyla"
}  
 
 
[{
    "overall": {
        "colors": {
            "White": {
                "coverage": 0.5834,
                "rgb": {
                    "red": 254,
                    "green": 254,
                    "blue": 243
                }
            },
            "Orange": {
                "coverage": 0.254,
                "rgb": {
                    "red": 249,
                    "green": 165,
                    "blue": 45
                }
            },
            "Gold": {
                "coverage": 0.0817,
                "rgb": {
                    "red": 253,
                    "green": 188,
                    "blue": 58
                }
            },
            "Mustard": {
                "coverage": 0.0727,
                "rgb": {
                    "red": 253,
                    "green": 207,
                    "blue": 84
                }
            },
            "Cream": {
                "coverage": 0.0082,
                "rgb": {
                    "red": 253,
                    "green": 236,
                    "blue": 174
                }
            }
        },
        "tones": {
            "warm": 0.4084,
            "neutral": 0.5916,
            "cool": 0
        }
    },
    "foreground": {
        "colors": {
            "Orange": {
                "coverage": 0.6022,
                "rgb": {
                    "red": 249,
                    "green": 165,
                    "blue": 45
                }
            },
            "Gold": {
                "coverage": 0.1935,
                "rgb": {
                    "red": 253,
                    "green": 188,
                    "blue": 58
                }
            },
            "Mustard": {
                "coverage": 0.1722,
                "rgb": {
                    "red": 253,
                    "green": 207,
                    "blue": 84
                }
            },
            "Cream": {
                "coverage": 0.0173,
                "rgb": {
                    "red": 253,
                    "green": 235,
                    "blue": 170
                }
            },
            "Yellow": {
                "coverage": 0.0148,
                "rgb": {
                    "red": 254,
                    "green": 229,
                    "blue": 117
                }
            }
        },
        "tones": {
            "warm": 0.9827,
            "neutral": 0.0173,
            "cool": 0
        }
    },
    "background": {
        "colors": {
            "White": {
                "coverage": 0.9923,
                "rgb": {
                    "red": 254,
                    "green": 254,
                    "blue": 243
                }
            },
            "Dark_Brown": {
                "coverage": 0.0077,
                "rgb": {
                    "red": 83,
                    "green": 68,
                    "blue": 57
                }
            }
        },
        "tones": {
            "warm": 0,
            "neutral": 1.0,
            "cool": 0
        }
    }
}]
```

イメージ全体の色に加えて、前景と背景の領域の色も確認できます。 上記の各領域に対してトーン検索が有効になっているので、トーンのヒストグラムを取り出すこともできます。

**入力パラメーター**

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| --- | --- | --- | --- | --- | --- |
| `documents` | 配列 (Document-Object) | ○ | - | 以下を参照してください。 | リスト内の各項目が 1 つのドキュメントを表す JSON 要素のリスト。 |
| `top_n` | 数値 | いいえ | 0 | 負でない整数 | 返される結果の数。 0：すべての結果を返します。 しきい値と組み合わせて使用した場合、返される結果の数は、どちらの制限よりも少なくなります。 |
| `min_coverage` | 数値 | いいえ | 0.05 | 実数 | 結果を返す必要があるカバレッジのしきい値。 すべての結果を返すには、Exclude パラメーターを使用します。 |
| `resize_image` | 数値 | いいえ | True | True/False | 入力イメージのサイズを変更するかどうか。 デフォルトでは、画像は色抽出が実行される前に 320*320 ピクセルにサイズ変更されます。 デバッグの目的で、をに設定して、フルイメージでもコードを実行できるようにします。 `False`. |
| `enable_mask` | 数値 | いいえ | False | True/False | カラー抽出を有効/無効にします |
| `retrieve_tone` | 数値 | いいえ | False | True/False | トーン抽出を有効/無効にします |

**Document オブジェクト**

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| -----| --------- | -------- | ------- | ------ | ----------- |
| `repo:path` | 文字列 | - | - | - | ドキュメントの署名済み URL。 |
| `sensei:repoType` | 文字列 | - | - | HTTPS | 画像が保存されるリポジトリのタイプ。 |
| `sensei:multipart_field_name` | 文字列 | - | - | - | 画像ファイルを、署名済みの URL を使用する代わりに、マルチパート引数として渡す場合に使用します。 |
| `dc:format` | 文字列 | ○ | - | &quot;image/jpg&quot;,<br>&quot;image/jpeg&quot;,<br>&quot;image/png&quot;,<br>&quot;image/tiff&quot; | 画像のエンコーディングは、処理前に、許可されている入力エンコーディングタイプと照合されます。 |