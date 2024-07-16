---
keywords: Experience Platform；はじめに；コンテンツ；コンテンツのタグ付け；カラータグ付け；カラー抽出；
solution: Experience Platform
title: コンテンツタグ付け API でのカラータグ付け
description: カラータグ付けサービスでは、画像を指定すると、ピクセルカラーのヒストグラムを計算し、支配的なカラーでグループに並べ替えることができます。
exl-id: 6b3b6314-cb67-404f-888c-4832d041f5ed
source-git-commit: fd8891bdc7d528e327d2a72c2427f7bbc6dc8a03
workflow-type: tm+mt
source-wordcount: '662'
ht-degree: 6%

---

# カラータグ付け

画像を指定すると、カラータグ付けサービスでピクセルカラーのヒストグラムを計算し、支配的なカラーでグループに並べ替えることができます。 画像ピクセル内の色は、色スペクトルを代表する 40 の支配的な色にバケット化される。 そして、これら 40 色の中から色値のヒストグラムを算出する。 このサービスには次の 2 つのバリアントがあります。

**カラータグ付け（フル画像）**

これにより、画像全体のカラーヒストグラムが抽出されます。

**カラータグ付け（マスク付き）**

このメソッドは、ディープラーニングベースのフォアグラウンド抽出を使用して、フォアグラウンド内のオブジェクトを識別します。 前景オブジェクトが抽出されると、前景領域と背景領域の両方の支配的な色について、画像全体と共にヒストグラムが計算されます。

**トーン抽出**

上記のバリエーションに加えて、以下のトーンのヒストグラムを取得するようにサービスを設定できます。

- 画像全体（完全な画像バリアントを使用する場合）
- 画像全体、前景および背景の領域（マスキングを付けたバリアントを使用する場合）

このドキュメントに示す例では、次の画像が使用されました。

![ テスト画像 ](../images/QQAsset1.jpg)

**API 形式**

```http
POST /services/v2/predict
```

**リクエスト – 完全な画像バリアント**

次のリクエスト例では、カラータグ付けに full-image メソッドを使用し、ペイロードに指定された入力パラメーターに基づいて画像から色を抽出します。 表示される入力パラメーターについて詳しくは、ペイロード例の下の表を参照してください。

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

**Response – 完全な画像バリアント**

応答が成功すると、抽出されたカラーの詳細が返されます。 各色は、次の情報を含む `feature_value` キーで表されます。

- カラー名
- この色が画像に対して表示される割合
- カラーのRGB値

`"White":{"coverage":0.5834,"rgb":{"red":254,"green":254,"blue":243}}` つまり、見つかった色は白で、画像の 58.34% に見られ、平均RGB値は 254、254、243 です。

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

ここでの結果は、「全体」の画像領域に抽出されたカラーであることに注意してください。

**リクエスト – マスクされた画像バリアント**

次のリクエスト例では、カラータグ付けにマスキング メソッドを使用しています。 これは、リクエストで `enable_mask` パラメーターを `true` に設定すると有効になります。

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
>さらに、上記のリクエストでは、`retrieve_tone` パラメーターも `true` に設定されています。 これにより、画像の全体、前景、背景の領域における、暖色系、中色系、冷色系のトーン分布ヒストグラムを取得できます。

**Response - マスクされた画像バリアント**

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

画像全体の色に加えて、前景と背景の領域の色も表示できるようになりました。 上記の各領域に対してトーン検索が有効になっているので、トーンのヒストグラムを検索することもできます。

**入力パラメーター**

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| --- | --- | --- | --- | --- | --- |
| `documents` | array （Document-Object） | ○ | - | 以下を参照 | リスト内の各項目が 1 つのドキュメントを表す JSON 要素のリスト。 |
| `top_n` | 数値 | × | 0 | 負でない整数 | 返される結果の数。 0 （すべての結果を返す）。 しきい値と共に使用すると、返される結果の数は、どちらの制限よりも小さくなります。 |
| `min_coverage` | 数値 | × | 0.05 | 実数 | 結果を返す必要があるカバレッジのしきい値。 すべての結果を返す除外パラメーター。 |
| `resize_image` | 数値 | × | True | True/False | 入力画像のサイズを変更するかどうか。 デフォルトでは、画像はカラー抽出が実行される前に 320 * 320 ピクセルにサイズ変更されます。 デバッグの目的で、これを `False` に設定して、コードを full-image でも実行できるようにすることができます。 |
| `enable_mask` | 数値 | × | False | True/False | カラー抽出を有効/無効にします |
| `retrieve_tone` | 数値 | × | False | True/False | トーン抽出を有効/無効にします |

**Document オブジェクト**

| 名前 | データタイプ | 必須 | デフォルト | 値 | 説明 |
| -----| --------- | -------- | ------- | ------ | ----------- |
| `repo:path` | 文字列 | - | - | - | ドキュメントの署名済み URL。 |
| `sensei:repoType` | 文字列 | - | - | HTTPS | 画像が保存されているリポジトリのタイプ。 |
| `sensei:multipart_field_name` | 文字列 | - | - | - | 画像ファイルをマルチパート引数として渡す場合、事前署名された URL を使用する代わりに、これを使用します。 |
| `dc:format` | 文字列 | ○ | - | &quot;image/jpg&quot;,<br>&quot;image/jpeg&quot;,<br>&quot;image/png&quot;,<br>&quot;image/tiff&quot; | 画像のエンコーディングは、処理前に、許可されている入力エンコーディングタイプと照合されます。 |