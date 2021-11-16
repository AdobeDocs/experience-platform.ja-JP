---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
title: ソース SDK の調査仕様の設定
topic-legacy: overview
description: このドキュメントでは、ソース SDK を使用するために準備が必要な設定の概要を説明します。
hide: true
hidefromtoc: true
source-git-commit: d4b5b54be9fa2b430a3b45eded94a523b6bd4ef8
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# ソース SDK の調査仕様の設定

エクスプローラ仕様は、ソースに含まれるオブジェクトの調査と検査に必要なパラメータを定義します。 また、エクスプローラの仕様では、オブジェクトの調査と検査時に返される応答形式も定義します。

>[!TIP]
>
>探索の仕様はハードコードされており、以下のペイロードをコピーして、接続の仕様に貼り付けるだけで済みます。

```json
"exploreSpec": {
  "name": "Resource",
  "type": "Resource",
  "requestSpec": {
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object"
  },
  "responseSpec": {
    "$schema": "http: //json-schema.org/draft-07/schema#",
    "type": "object",
    "properties": {
      "format": {
        "type": "string"
      },
      "schema": {
        "type": "object",
        "properties": {
          "columns": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "string"
                },
                "type": {
                  "type": "string"
                }
              }
            }
          }
        }
      },
      "data": {
        "type": "array",
        "items": {
          "type": "object"
        }
      }
    }
  }
}
```

| 仕様の調査 | 説明 | 例 |
| --- | --- | --- |
| `name` | エクスプローラ仕様の名前または識別子を定義します。 | `Resource` |
| `type` | エクスプローラ仕様のタイプを定義します。 | `Resource` |
| `requestSpec` | 接続内のオブジェクトの探索に必要なパラメータを含みます。 |
| `requestSpec.type` | リクエスト仕様のデータタイプを定義します。 | `object` |
| `responseSpec` | 探索呼び出しに対して返される応答メッセージの形式を定義するパラメーターを含みます。 |
| `responseSpec.type` | 応答仕様のデータタイプを定義します。 | `object` |
| `responseSpec.properties` | 応答メッセージの形式に関する情報が含まれます。 |
| `responseSpec.properties.format` | 応答スキーマの形式を定義します。 | `object` |
| `responseSpec.properties.format.type` | プロパティのデータタイプを定義します。 | `string` |
| `responseSpec.schema` | 応答スキーマの形式に関する情報が含まれます。 |
| `responseSpec.schema.type` | スキーマのデータ型を定義します。 | `object` |
| `responseSpec.schema.properties` | スキーマ内で保持される列、タイプ、項目に関する情報が含まれます。 |
| `responseSpec.schema.properties.columns.items.properties.name` | ファイルの名前を表示します。 |
| `responseSpec.schema.properties.columns.items.properties.name.type` | ファイル名のデータタイプを定義します。 | `string` |

{style=&quot;table-layout:auto&quot;}

## 次の手順

エクスプローラ仕様を入力したら、次を使用して完全な接続仕様を作成できます： [!DNL Flow Service] API 詳しくは、 [[!DNL Sources SDK] API ガイド](../api/overview.md) を参照してください。