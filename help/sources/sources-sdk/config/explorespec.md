---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: セルフサービスソースの探索仕様の設定（Batch SDK）
description: このドキュメントでは、セルフサービスソース（Batch SDK）を使用するために準備が必要な設定の概要を説明します。
exl-id: 423a7e56-9dd1-4071-bd26-ee4f9f206122
source-git-commit: 16cc811a545414021b8686ae303d6112bcf6cebb
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 6%

---

# セルフサービスソースの探索仕様の設定（Batch SDK）

「探索仕様」では、ソースに含まれるオブジェクトを探索および検査するために必要なパラメータを定義します。 また、探索仕様では、オブジェクトの探索および検査時に返される応答形式も定義します。

>[!TIP]
>
>参照仕様はハードコードされており、以下のペイロードを接続仕様にコピー&amp;ペーストするだけで使用できます。

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

| 仕様を調べる | 説明 | 例 |
| --- | --- | --- |
| `name` | 参照仕様の名前または識別子を定義します。 | `Resource` |
| `type` | 参照仕様のタイプを定義します。 | `Resource` |
| `requestSpec` | 接続内のオブジェクトを探索するために必要なパラメーターが含まれます。 |  |
| `requestSpec.type` | リクエスト仕様のデータタイプを定義します。 | `object` |
| `responseSpec` | 探索呼び出しに対して返される応答メッセージの形式を定義するパラメーターが含まれます。 |  |
| `responseSpec.type` | 応答の仕様のデータタイプを定義します。 | `object` |
| `responseSpec.properties` | 応答メッセージの形式に関する情報が含まれます。 |  |
| `responseSpec.properties.format` | 応答スキーマのフォーマットを定義します。 | `object` |
| `responseSpec.properties.format.type` | プロパティのデータタイプを定義します。 | `string` |
| `responseSpec.schema` | 応答スキーマの形式に関する情報が含まれます。 |  |
| `responseSpec.schema.type` | スキーマのデータタイプを定義します。 | `object` |
| `responseSpec.schema.properties` | スキーマ内に保持されている列、タイプ、項目に関する情報が含まれます。 |  |
| `responseSpec.schema.properties.columns.items.properties.name` | ファイルの名前が表示されます。 |  |
| `responseSpec.schema.properties.columns.items.properties.name.type` | ファイル名のデータタイプを定義します。 | `string` |

{style="table-layout:auto"}

## 次の手順

探索仕様を入力したら、[!DNL Flow Service] API を使用して完全な接続仕様を作成できます。 詳しくは、[Self-Serve Sources （バッチ SDK） API ガイド &#x200B;](../api/api-overview.md) を参照してください。
