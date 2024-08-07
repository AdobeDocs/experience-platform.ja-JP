---
keywords: Experience Platform；ホーム；人気のトピック；flow service;Flow Service API；ソース；ソース
title: Flow Service API を使用したSourceの行レベルのデータのフィルタリング
description: このチュートリアルでは、Flow Service API を使用してソースレベルでデータをフィルタリングする手順を説明します
exl-id: 224b454e-a079-4df3-a8b2-1bebfb37d11f
source-git-commit: b0e2fc4767fb6fbc90bcdd3350b3add965988f8f
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 14%

---

# [!DNL Flow Service] API を使用したソースの行レベルのデータのフィルタリング

>[!IMPORTANT]
>
>行レベルのデータのフィルタリングのサポートは、現在、次のソースでのみ使用できます。
>
>* [Google BigQuery](../../connectors/databases/bigquery.md)
>* [Microsoft Dynamics](../../connectors/crm/ms-dynamics.md)
>* [Salesforce](../../connectors/crm/salesforce.md)
>* [Snowflake](../../connectors/databases/snowflake.md)

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してソースの行レベルのデータをフィルタリングする手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## ソースデータをフィルター

ソースの行レベルのデータをフィルタリングするための手順の概要を次に示します。

### 接続仕様の検索

API を使用してソースの行レベルのデータをフィルタリングする前に、まずソースの接続仕様の詳細を取得して、特定のソースがサポートする演算子と言語を決定する必要があります。

特定のソースの接続仕様を取得するには、[!DNL Flow Service] API の `/connectionSpecs` エンドポイントにGETリクエストを行い、その際にソースのプロパティ名をクエリパラメーターの一部として指定します。

**API 形式**

```http
GET /connectionSpecs/{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 [!DNL Google BigQuery] 接続仕様を取得するには、`name` プロパティを適用して、検索で `"google-big-query"` を指定します。 |

**リクエスト**

次のリクエストは、[!DNL Google BigQuery] の接続仕様を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="google-big-query"' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、サポートされるクエリ言語や論理演算子の情報を含め、[!DNL Google BigQuery] の接続仕様が返されます。

>[!NOTE]
>
>以下の API 応答は、簡潔にするために切り捨てられています。

```json
"attributes": {
  "filterAtSource": {
    "enabled": true,
    "queryLanguage": "SQL",
    "logicalOperators": [
      "and",
      "or",
      "not"
    ],
    "comparisonOperators": [
      "=",
      "!=",
      "<",
      "<=",
      ">",
      ">=",
      "like",
      "in"
    ],
    "columnNameEscapeChar": "`",
    "valueEscapeChar": "'"
  }
```

| プロパティ | 説明 |
| --- | --- |
| `attributes.filterAtSource.enabled` | クエリされたソースが行レベルのデータのフィルタリングをサポートしているかどうかを判断します。 |
| `attributes.filterAtSource.queryLanguage` | クエリされたソースがサポートするクエリ言語を決定します。 |
| `attributes.filterAtSource.logicalOperators` | ソースの行レベルのデータをフィルタリングするために使用できる論理演算子を決定します。 |
| `attributes.filterAtSource.comparisonOperators` | ソースの行レベルのデータをフィルタリングするために使用できる比較演算子を決定します。 比較演算子について詳しくは、次の表を参照してください。 |
| `attributes.filterAtSource.columnNameEscapeChar` | 列のエスケープに使用する文字を決定します。 |
| `attributes.filterAtSource.valueEscapeChar` | SQL クエリを記述するときに値をどのように囲むかを指定します。 |

{style="table-layout:auto"}

#### 比較演算子

| 演算子 | 説明 |
| --- | --- |
| `==` | プロパティが指定された値に等しいかどうかを基準にフィルタリングします。 |
| `!=` | プロパティが指定された値に等しくないかどうかをフィルタリングします。 |
| `<` | プロパティが指定された値より小さいかどうかを基準にフィルタリングします。 |
| `>` | プロパティが指定された値より大きいかどうかを基準にフィルタリングします。 |
| `<=` | プロパティが指定された値以下であるかによってフィルタリングします。 |
| `>=` | プロパティが指定された値以上であるかによってフィルタリングします。 |
| `like` | `WHERE` 句で使用され、指定したパターンを検索することでフィルタリングします。 |
| `in` | プロパティが指定した範囲内にあるかどうかを基準にフィルタを適用します。 |

{style="table-layout:auto"}

### 取り込みのフィルター条件を指定

ソースがサポートする論理演算子とクエリ言語を識別したら、Profile Query Language（PQL）を使用して、ソースデータに適用するフィルター条件を指定できます。

以下の例では、パラメーターとしてリストされているノードタイプに指定された値と等しいデータのみを選択するために条件が適用されます。

```json
{
  "type": "PQL",
  "format": "pql/json",
  "value": {
    "nodeType": "fnApply",
    "fnName": "=",
    "params": [
      {
        "nodeType": "fieldLookup",
        "fieldName": "city"
      },
      {
        "nodeType": "literal",
        "value": "DDN"
      }
    ]
  }
}
```

### データのプレビュー

[!DNL Flow Service] API の `/explore` エンドポイントに対してデータリクエストを行い、その際にクエリパラメーターの一部として `filters` を指定し、[!DNL Base64] でPQL入力条件を指定することで、GETをプレビューできます。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}&preview=true&filters={FILTERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ソースのベース接続 ID。 |
| `{TABLE_PATH}` | 検査するテーブルのパスプロパティ。 |
| `{FILTERS}` | PQLのフィルター条件が [!DNL Base64] でエンコードされている。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/89d1459e-3cd0-4069-acb3-68f240db4eeb/explore?objectType=table&object=TESTFAS.FASTABLE&preview=true&filters=ewogICJ0eXBlIjogIlBRTCIsCiAgImZvcm1hdCI6ICJwcWwvanNvbiIsCiAgInZhbHVlIjogewogICAgIm5vZGVUeXBlIjogImZuQXBwbHkiLAogICAgImZuTmFtZSI6ICJhbmQiLAogICAgInBhcmFtcyI6IFsKICAgICAgewogICAgICAgICJub2RlVHlwZSI6ICJmbkFwcGx5IiwKICAgICAgICAiZm5OYW1lIjogImxpa2UiLAogICAgICAgICJwYXJhbXMiOiBbCiAgICAgICAgICB7CiAgICAgICAgICAgICJub2RlVHlwZSI6ICJmaWVsZExvb2t1cCIsCiAgICAgICAgICAgICJmaWVsZE5hbWUiOiAiY2l0eSIKICAgICAgICAgIH0sCiAgICAgICAgICB7CiAgICAgICAgICAgICJub2RlVHlwZSI6ICJsaXRlcmFsIiwKICAgICAgICAgICAgInZhbHVlIjogIk0lIgogICAgICAgICAgfQogICAgICAgIF0KICAgICAgfQogICAgXQogIH0KfQ==\' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功すると、次の応答が返されます。

```json
{
  "format": "flat",
  "schema": {
    "columns": [
      {
        "name": "FIRSTNAME",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "LASTNAME",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "CITY",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "AGE",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "HEIGHT",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "ISEMPLOYED",
        "type": "boolean",
        "xdm": {
          "type": "boolean"
        }
      },
      {
        "name": "POSTG",
        "type": "boolean",
        "xdm": {
          "type": "boolean"
        }
      },
      {
        "name": "LATITUDE",
        "type": "double",
        "xdm": {
          "type": "number"
        }
      },
      {
        "name": "LONGITUDE",
        "type": "double",
        "xdm": {
          "type": "number"
        }
      },
      {
        "name": "JOINEDDATE",
        "type": "string",
        "meta:xdmType": "date-time",
        "xdm": {
          "type": "string",
          "format": "date-time"
        }
      },
      {
        "name": "CREATEDAT",
        "type": "string",
        "meta:xdmType": "date-time",
        "xdm": {
          "type": "string",
          "format": "date-time"
        }
      },
      {
        "name": "CREATEDATTS",
        "type": "string",
        "meta:xdmType": "date-time",
        "xdm": {
          "type": "string",
          "format": "date-time"
        }
      }
    ]
  },
 "data": [
    {
        "CITY": "MZN",
        "LASTNAME": "Jain",
        "JOINEDDATE": "2022-06-22T00:00:00",
        "LONGITUDE": 1000.222,
        "CREATEDAT": "2022-06-22T17:19:33",
        "FIRSTNAME": "Shivam",
        "POSTG": true,
        "HEIGHT": "169",
        "CREATEDATTS": "2022-06-22T17:19:33",
        "ISEMPLOYED": true,
        "LATITUDE": 2000.89,
        "AGE": "25"
    },
    {
        "CITY": "MUM",
        "LASTNAME": "Kreet",
        "JOINEDDATE": "2022-09-07T00:00:00",
        "LONGITUDE": 10500.01,
        "CREATEDAT": "2022-09-07T17:19:33",
        "FIRSTNAME": "Rakul",
        "POSTG": true,
        "HEIGHT": "155",
        "CREATEDATTS": "2022-09-07T17:19:33",
        "ISEMPLOYED": false,
        "LATITUDE": 2500.89,
        "AGE": "42"
    },
    {
        "CITY": "MAN",
        "LASTNAME": "Lee",
        "JOINEDDATE": "2022-09-14T00:00:00",
        "LONGITUDE": 1000.222,
        "CREATEDAT": "2022-09-14T05:02:33",
        "FIRSTNAME": "Denzel",
        "POSTG": true,
        "HEIGHT": "185",
        "CREATEDATTS": "2022-09-14T05:02:33",
        "ISEMPLOYED": true,
        "LATITUDE": 123.89,
        "AGE": "16"
    }
  ]
}
```

### フィルターされたデータのソース接続の作成

ソース接続を作成し、フィルターされたデータを取り込むには、`/sourceConnections` エンドポイントに対してPOSTリクエストを実行し、その際にフィルタリング条件を本文パラメーターの一部として指定します。

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

次のリクエストでは、`city` = `DDN` の `test1.fasTestTable` からデータを取り込むソース接続を作成しています。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "BigQuery Source Connection",
      "description": "Source Connection for Filter test",
      "baseConnectionId": "89d1459e-3cd0-4069-acb3-68f240db4eeb",
      "data": {
        "format": "tabular"
      },
      "params": {
        "tableName": "test1.fasTestTable",
        "filters": {
          "type": "PQL",
          "format": "pql/json",
          "value": {
            "nodeType": "fnApply",
            "fnName": "=",
            "params": [
              {
                "nodeType": "fieldLookup",
                "fieldName": "city"
              },
              {
                "nodeType": "literal",
                "value": "DDN"
              }
            ]
          }
        }
      },
      "connectionSpec": {
        "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
        "version": "1.0"
      }
    }'
```

**応答**

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID （`id`）が返されます。

```json
{
    "id": "b7581b59-c603-4df1-a689-d23d7ac440f3",
    "etag": "\"ef05d265-0000-0200-0000-6019e0080000\""
}
```

## 付録

この節では、フィルタリングに使用する様々なペイロードの例をさらに示します。

### 特異条件

1 つの条件のみを必要とするシナリオでは、初期 `fnApply` を省略できます。

```json
{
  "type": "PQL",
  "format": "pql/json",
  "value": {
    "nodeType": "fnApply",
    "fnName": "like",
    "params": [
      {
        "nodeType": "fieldLookup",
        "fieldName": "firstname"
      },
      {
        "nodeType": "literal",
        "value": "%s"
      }
    ]
  }
}
```

### `in` 演算子の使用

演算子 `in` の例については、以下のサンプルペイロードを参照してください。

```json
{
  "type": "PQL",
  "format": "pql/json",
  "value": {
    "nodeType": "fnApply",
    "fnName": "and",
    "params": [
      {
        "nodeType": "fnApply",
        "fnName": "in",
        "params": [
          {
            "nodeType": "fieldLookup",
            "fieldName": "firstname"
          },
          {
            "nodeType": "literal",
            "value": [
              "Ramen",
              "John"
            ]
          }
        ]
      }
    ]
  }
}
```

### `isNull` 演算子の使用

演算子 `isNull` の例については、以下のサンプルペイロードを参照してください。

```json
{
  "type": "PQL",
  "format": "pql/json",
  "value": {
    "nodeType": "fnApply",
    "fnName": "isNull",
    "params": [
      {
        "nodeType": "fieldLookup",
        "fieldName": "complaint_type"
      }
    ]
  }
}
```

### `NOT` 演算子の使用

演算子 `NOT` の例については、以下のサンプルペイロードを参照してください。

```json
{
  "type": "PQL",
  "format": "pql/json",
  "value": {
    "nodeType": "fnApply",
    "fnName": "NOT",
    "params": [
      {
        "nodeType": "fnApply",
        "fnName": "isNull",
        "params": [
          {
            "nodeType": "fieldLookup",
            "fieldName": "complaint_type"
          }
        ]
      }
    ]
  }
}
```

### ネストされた条件を使用した例

複雑なネスト条件の例については、以下のサンプルペイロードを参照してください。

```json
{
  "type": "PQL",
  "format": "pql/json",
  "value": {
    "nodeType": "fnApply",
    "fnName": "and",
    "params": [
      {
        "nodeType": "fnApply",
        "fnName": ">=",
        "params": [
          {
            "nodeType": "fieldLookup",
            "fieldName": "age"
          },
          {
            "nodeType": "literal",
            "value": 20
          }
        ]
      },
      {
        "nodeType": "fnApply",
        "fnName": "<=",
        "params": [
          {
            "nodeType": "fieldLookup",
            "fieldName": "age"
          },
          {
            "nodeType": "literal",
            "value": 30
          }
        ]
      },
      {
        "nodeType": "fnApply",
        "fnName": "or",
        "params": [
          {
            "nodeType": "fnApply",
            "fnName": "!=",
            "params": [
              {
                "nodeType": "fieldLookup",
                "fieldName": "city"
              },
              {
                "nodeType": "literal",
                "value": "PUD"
              }
            ]
          },
          {
            "nodeType": "fnApply",
            "fnName": "=",
            "params": [
              {
                "nodeType": "fieldLookup",
                "fieldName": "joinedDate"
              },
              {
                "nodeType": "literal",
                "value": "2020-04-22"
              }
            ]
          }
        ]
      }
    ]
  }
}
```
