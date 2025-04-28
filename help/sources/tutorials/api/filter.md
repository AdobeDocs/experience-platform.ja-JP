---
title: Flow Service API を使用したSourceの行レベルのデータのフィルタリング
description: このチュートリアルでは、Flow Service API を使用してソースレベルでデータをフィルタリングする手順を説明します
exl-id: 224b454e-a079-4df3-a8b2-1bebfb37d11f
source-git-commit: 67abd5cda9cff1da8757ef691ebbf27e9a5550c5
workflow-type: tm+mt
source-wordcount: '1825'
ht-degree: 15%

---

# [!DNL Flow Service] API を使用したソースの行レベルのデータのフィルタリング

>[!AVAILABILITY]
>
>行レベルのデータのフィルタリングのサポートは、現在、次のソースでのみ使用できます。
>
>* [[Amazon Redshift]](../../connectors/databases/redshift.md)
>* [[!DNL Google BigQuery]](../../connectors/databases/bigquery.md)
>* [[!DNL Marketo Engage]  標準アクティビティ ](../../connectors/adobe-applications/marketo/marketo.md)
>* [[!DNL Microsoft Dynamics]](../../connectors/crm/ms-dynamics.md)
>* [[!DNL Salesforce]](../../connectors/crm/salesforce.md)
>* [[!DNL Snowflake]](../../connectors/databases/snowflake.md)

[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してソースの行レベルのデータをフィルタリングする手順については、このガイドを参照してください。

## 基本を学ぶ

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../landing/api-guide.md) を参照してください。

## ソースデータをフィルター {#filter-source-data}

ソースの行レベルのデータをフィルタリングするための手順の概要を次に示します。

### 接続仕様の取得 {#retrieve-your-connection-specs}

ソースの行レベルのデータをフィルタリングする最初の手順は、ソースの接続仕様を取得し、ソースがサポートする演算子と言語を決定することです。

特定のソースの接続仕様を取得するには、[!DNL Flow Service] API の `/connectionSpecs` エンドポイントに対してGET リクエストを実行し、クエリパラメーターの一部としてソースのプロパティ名を指定します。

**API 形式**

```http
GET /connectionSpecs/{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 `name` プロパティを適用して検索で `"google-big-query"` を指定すると、[!DNL Google BigQuery] 接続仕様を取得できます。 |

+++リクエスト

次のリクエストは、[!DNL Google BigQuery] の接続仕様を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="google-big-query"' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

+++応答

応答が成功すると、ステータスコード 200 と、サポートされるクエリ言語と論理演算子の情報を含む、[!DNL Google BigQuery] の接続仕様が返されます。

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

+++

#### 比較演算子 {#comparison-operators}

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

### 取り込みのフィルター条件を指定 {#specify-filtering-conditions-for-ingestion}

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

### データのプレビュー {#preview-your-data}

[!DNL Flow Service] API の `/explore` エンドポイントに対してGET リクエストを行い、その際にクエリパラメーターの一部として `filters` を指定し、[!DNL Base64] でPQL入力条件を指定することで、データをプレビューできます。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}&preview=true&filters={FILTERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ソースのベース接続 ID。 |
| `{TABLE_PATH}` | 検査するテーブルのパスプロパティ。 |
| `{FILTERS}` | PQLのフィルター条件が [!DNL Base64] でエンコードされている。 |

+++リクエスト

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/89d1459e-3cd0-4069-acb3-68f240db4eeb/explore?objectType=table&object=TESTFAS.FASTABLE&preview=true&filters=ewogICJ0eXBlIjogIlBRTCIsCiAgImZvcm1hdCI6ICJwcWwvanNvbiIsCiAgInZhbHVlIjogewogICAgIm5vZGVUeXBlIjogImZuQXBwbHkiLAogICAgImZuTmFtZSI6ICJhbmQiLAogICAgInBhcmFtcyI6IFsKICAgICAgewogICAgICAgICJub2RlVHlwZSI6ICJmbkFwcGx5IiwKICAgICAgICAiZm5OYW1lIjogImxpa2UiLAogICAgICAgICJwYXJhbXMiOiBbCiAgICAgICAgICB7CiAgICAgICAgICAgICJub2RlVHlwZSI6ICJmaWVsZExvb2t1cCIsCiAgICAgICAgICAgICJmaWVsZE5hbWUiOiAiY2l0eSIKICAgICAgICAgIH0sCiAgICAgICAgICB7CiAgICAgICAgICAgICJub2RlVHlwZSI6ICJsaXRlcmFsIiwKICAgICAgICAgICAgInZhbHVlIjogIk0lIgogICAgICAgICAgfQogICAgICAgIF0KICAgICAgfQogICAgXQogIH0KfQ==\' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

+++応答

応答が成功すると、データの内容と構造が返されます。

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

+++

### フィルターされたデータのソース接続の作成

ソース接続を作成し、フィルターされたデータを取り込むには、`/sourceConnections` エンドポイントに対して POST リクエストを実行し、リクエスト本文のパラメーターにフィルター条件を指定します。

**API 形式**

```http
POST /sourceConnections
```

+++リクエスト

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

+++

+++応答

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID （`id`）が返されます。

```json
{
    "id": "b7581b59-c603-4df1-a689-d23d7ac440f3",
    "etag": "\"ef05d265-0000-0200-0000-6019e0080000\""
}
```

+++

## [!DNL Marketo Engage] のアクティビティエンティティのフィルタリング {#filter-for-marketo}

[[!DNL Marketo Engage]  ソースコネクタ ](../../connectors/adobe-applications/marketo/marketo.md) を使用する場合、行レベルのフィルタリングを使用して、アクティビティエンティティをフィルタリングできます。 現在、フィルターできるのは、アクティビティエンティティと標準アクティビティタイプのみです。 カスタムアクティビティは、引き続き [[!DNL Marketo]  フィールドマッピング ](../../connectors/adobe-applications/mapping/marketo.md) で管理されます。

### [!DNL Marketo] 標準アクティビティタイプ {#marketo-standard-activity-types}

次の表に、[!DNL Marketo] の標準アクティビティタイプの概要を示します。 このテーブルをフィルター条件の参照として使用します。

| アクティビティタイプ ID | アクティビティタイプ名 |
| --- | --- |
| 1 | Web ページにアクセス |
| 2 | フォームに入力 |
| 3 | リンクをクリック |
| 6 | メールを送信 |
| 7 | 電子メール配信済み |
| 8 | 電子メールバウンス |
| 9 | メールの登録解除 |
| 10 | メールを開く |
| 11 | Click Email |
| 12 | 新しいリード |
| 21 | リードを変換 |
| 22 | スコアを変更 |
| 24 | リストに追加 |
| 25 | リストから削除 |
| 27 | 電子メールバウンス (ソフト) |
| 32 | リードを結合 |
| 34 | オポチュニティに追加 |
| 35 | オポチュニティから削除 |
| 36 | オポチュニティを更新 |
| 46 | 興味深い瞬間 |
| 101 | 収益ステージを変更 |
| 104 | 進行状況のステータスを変更 |
| 110 | Webhook を呼び出す |
| 113 | ナーチャリングに追加 |
| 114 | ナーチャリングトラックを変更 |
| 115 | ナーチャリング頻度を変更 |

{style="table-layout:auto"}

[!DNL Marketo] ソースコネクタを使用する場合は、次の手順に従って標準アクティビティエンティティをフィルタリングします。

### ドラフトデータフローの作成

まず、[[!DNL Marketo]  データフロー ](../ui/create/adobe-applications/marketo.md) を作成し、ドラフトとして保存します。 ドラフトデータフローの作成方法に関する詳細な手順については、次のドキュメントを参照してください。

* [UI を使用したデータフローのドラフトでの保存](../ui/draft.md)
* [API を使用したデータフローのドラフトでの保存](../api/draft.md)

### データフロー ID の取得

データフローのドラフトを作成したら、対応する ID を取得する必要があります。

UI でソースカタログに移動し、上部のヘッダーから **[!UICONTROL データフロー]** を選択します。 ステータス列を使用して、ドラフトモードで保存されたすべてのデータフローを識別し、データフローの名前を選択します。 次に、右側の **[!UICONTROL プロパティ]** パネルを使用して、データフロー ID を見つけます。

### データフローの詳細の取得

次に、データフローの詳細（特に、データフローに関連付けられたソース接続 ID）を取得する必要があります。 データフローの詳細を取得するには、`/flows` エンドポイントに対してGET リクエストを実行し、データフロー ID をパスパラメーターとして指定します。

**API 形式**

```http
GET /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FLOW_ID}` | 取得するデータフローの ID。 |

+++リクエスト

次のリクエストでは、データフロー ID `a7e88a01-40f9-4ebf-80b2-0fc838ff82ef` に関する情報を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/flows/a7e88a01-40f9-4ebf-80b2-0fc838ff82ef' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

+++応答

応答が成功すると、データフローの詳細（対応するソース接続とターゲット接続に関する情報を含む）が返されます。 データフローを公開するにはソース接続 ID とターゲット接続 ID が後で必要になるので、これらの値をメモしておく必要があります。

```json {line-numbers="true" start-line="1" highlight="23, 26"}
{
    "items": [
        {
            "id": "a7e88a01-40f9-4ebf-80b2-0fc838ff82ef",
            "createdAt": 1728592929650,
            "updatedAt": 1728597187444,
            "createdBy": "acme@AdobeID",
            "updatedBy": "acme@AdobeID",
            "createdClient": "exc_app",
            "updatedClient": "acme",
            "sandboxId": "7f3419ce-53e2-476b-b419-ce53e2376b02",
            "sandboxName": "prod",
            "imsOrgId": "acme@AdobeOrg",
            "name": "Marketo Engage Standard Activities ACME",
            "description": "",
            "flowSpec": {
                "id": "15f8402c-ba66-4626-b54c-9f8e54244d61",
                "version": "1.0"
            },
            "state": "enabled",
            "version": "\"600290fc-0000-0200-0000-67084cc30000\"",
            "etag": "\"600290fc-0000-0200-0000-67084cc30000\"",
            "sourceConnectionIds": [
                "56f7eb3a-b544-4eaa-b167-ef1711044c7a"
            ],
            "targetConnectionIds": [
                "7e53e6e8-b432-4134-bb29-21fc6e8532e5"
            ],
            "inheritedAttributes": {
                "properties": {
                    "isSourceFlow": true
                },
                "sourceConnections": [
                    {
                        "id": "56f7eb3a-b544-4eaa-b167-ef1711044c7a",
                        "connectionSpec": {
                            "id": "bf1f4218-73ce-4ff0-b744-48d78ffae2e4",
                            "version": "1.0"
                        },
                        "baseConnection": {
                            "id": "0137118b-373a-4c4e-847c-13a0abf73b33",
                            "connectionSpec": {
                                "id": "bf1f4218-73ce-4ff0-b744-48d78ffae2e4",
                                "version": "1.0"
                            }
                        }
                    }
                ],
                "targetConnections": [
                    {
                        "id": "7e53e6e8-b432-4134-bb29-21fc6e8532e5",
                        "connectionSpec": {
                            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
                            "version": "1.0"
                        }
                    }
                ]
            },
            "options": {
                "isSampleDataflow": false,
                "errorDiagnosticsEnabled": true
            },
            "transformations": [
                {
                    "name": "Mapping",
                    "params": {
                        "mappingVersion": 0,
                        "mappingId": "f6447514ef95482889fac1818972e285"
                    }
                }
            ],
            "runs": "/runs?property=flowId==a7e88a01-40f9-4ebf-80b2-0fc838ff82ef",
            "lastOperation": {
                "started": 1728592929650,
                "updated": 0,
                "operation": "create"
            },
            "lastRunDetails": {
                "id": "2d7863d5-ca4d-4313-ac52-2603eaf2cdbe",
                "state": "success",
                "startedAtUTC": 1728594713537,
                "completedAtUTC": 1728597183080
            },
            "labels": [],
            "recordTypes": [
                {
                    "type": "experienceevent",
                    "extensions": {}
                }
            ]
        }
    ]
}
```

+++

### ソース接続の詳細の取得

次に、ソース接続 ID を使用し、`/sourceConnections` エンドポイントに対してGET リクエストを実行して、ソース接続の詳細を取得します。

**API 形式**

```http
GET /sourceConnections/{SOURCE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SOURCE_CONNECTION_ID}` | 取得するソース接続の ID。 |

+++リクエスト

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections/56f7eb3a-b544-4eaa-b167-ef1711044c7a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

+++応答

応答が成功すると、ソース接続の詳細が返されます。 ソース接続を更新するために、次の手順でこの値が必要になるので、バージョンをメモしておきます。

```json {line-numbers="true" start-line="1" highlight="30"}
{
    "items": [
        {
            "id": "b85b895f-a289-42e9-8fe1-ae448ccc7e53",
            "createdAt": 1729634331185,
            "updatedAt": 1729634331185,
            "createdBy": "acme@AdobeID",
            "updatedBy": "acme@AdobeID",
            "createdClient": "exc_app",
            "updatedClient": "acme",
            "sandboxId": "7f3419ce-53e2-476b-b419-ce53e2376b02",
            "sandboxName": "prod",
            "imsOrgId": "acme@AdobeOrg",
            "name": "New Source Connection - 2024-10-23T03:28:50+05:30",
            "description": "Source connection created from the workflow",
            "baseConnectionId": "fd9f7455-1e23-4831-9283-7717e20bee40",
            "state": "draft",
            "data": {
                "format": "tabular",
                "schema": null,
                "properties": null
            },
            "connectionSpec": {
                "id": "2d31dfd1-df1a-456b-948f-226e040ba102",
                "version": "1.0"
            },
            "params": {
                "columns": [],
                "tableName": "Activity"
            },
            "version": "\"210068a6-0000-0200-0000-6718201b0000\"",
            "etag": "\"210068a6-0000-0200-0000-6718201b0000\"",
            "inheritedAttributes": {
                "baseConnection": {
                    "id": "fd9f7455-1e23-4831-9283-7717e20bee40",
                    "connectionSpec": {
                        "id": "2d31dfd1-df1a-456b-948f-226e040ba102",
                        "version": "1.0"
                    }
                }
            },
            "lastOperation": {
                "started": 1729634331185,
                "updated": 0,
                "operation": "draft_create"
            }
        }
    ]
}
```

+++

### フィルター条件によるソース接続の更新

ソース接続 ID と対応するバージョンが用意できたので、標準のアクティビティタイプを指定するフィルター条件を使用してPATCH リクエストを実行できます。

ソース接続を更新するには、`/sourceConnections` エンドポイントに対してPATCH リクエストを実行し、ソース接続 ID をクエリパラメーターとして指定します。 さらに、`If-Match` ヘッダーパラメーターと、ソース接続の対応するバージョンを指定する必要があります。

>[!TIP]
>
>`If-Match` ヘッダーは、PATCH リクエストを行う際に必要です。このヘッダーの値は、更新するデータフローの一意のバージョン/etag です。 version/etag の値は、データフローが正常に更新されるたびに更新されます。

**API 形式**

```http
PATCH /sourceConnections/{SOURCE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SOURCE_CONNECTION_ID}` | 更新するソース接続の ID |

+++リクエスト

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections/56f7eb3a-b544-4eaa-b167-ef1711044c7a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'If-Match: {VERSION_HERE}'
  -d '
      {
        "op": "add",
        "path": "/params/filters",
        "value": {
            "type": "PQL",
            "format": "pql/json",
            "value": {
                "nodeType": "fnApply",
                "fnName": "in",
                "params": [
                    {
                        "nodeType": "fieldLookup",
                        "fieldName": "activityType"
                    },
                    {
                        "nodeType": "literal",
                        "value": [
                            "Change Status in Progression",
                            "Fill Out Form"
                        ]
                    }
                ]
            }
        }
    }'
```

+++

+++応答

正常な応答は、ソース接続 ID と etag （バージョン）を返します。

```json
{
    "id": "56f7eb3a-b544-4eaa-b167-ef1711044c7a",
    "etag": "\"210068a6-0000-0200-0000-6718201b0000\""
}
```

+++

### ソース接続の公開

フィルター条件を使用してソース接続を更新すると、ドラフト状態から進んでソース接続を公開できるようになります。 これを行うには、`/sourceConnections` エンドポイントに対して POST リクエストを実行し、ドラフトソース接続の ID と公開用のアクション操作を指定します。

**API 形式**

```http
POST /sourceConnections/{SOURCE_CONNECTION_ID}/action?op=publish
```

| パラメーター | 説明 |
| --- | --- |
| `{SOURCE_CONNECTION_ID}` | 公開するソース接続の ID。 |
| `op` | クエリされたソース接続の状態を更新するアクション操作。ドラフトソース接続を公開するには、`op` を `publish` に設定します。 |

+++リクエスト

次のリクエストでは、ドラフトに設定したソース接続を公開しています。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections/56f7eb3a-b544-4eaa-b167-ef1711044c7a/action?op=publish' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

+++応答

正常な応答は、ソース接続 ID と etag （バージョン）を返します。

```json
{
    "id": "56f7eb3a-b544-4eaa-b167-ef1711044c7a",
    "etag": "\"9f007f7b-0000-0200-0000-670ef1150000\""
}
```

+++

### ターゲット接続の公開

前のステップと同様に、ドラフトデータフローを続行して公開するには、ターゲット接続も公開する必要があります。 `/targetConnections` エンドポイントに対して POST リクエストを実行し、公開するドラフトターゲット接続の ID と公開用のアクション操作を指定します。

**API 形式**

```http
POST /targetConnections/{TARGET_CONNECTION_ID}/action?op=publish
```

| パラメーター | 説明 |
| --- | --- |
| `{TARGET_CONNECTION_ID}` | 公開するターゲット接続の ID。 |
| `op` | クエリされたターゲット接続の状態を更新するアクション操作。ドラフトターゲット接続を公開するには、`op` を `publish` に設定します。 |

+++リクエスト

次のリクエストでは、ID `7e53e6e8-b432-4134-bb29-21fc6e8532e5` のターゲット接続が公開されます。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections/7e53e6e8-b432-4134-bb29-21fc6e8532e5/action?op=publish' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

+++応答

正常な応答は、ID と、公開したターゲット接続に対応する etag を返します。

```json
{
    "id": "7e53e6e8-b432-4134-bb29-21fc6e8532e5",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

+++


### データフローの公開

ソース接続とターゲット接続の両方が公開されたので、最後の手順に進み、データフローを公開できます。 データフローを公開するには、`/flows` エンドポイントに対して POST リクエストを実行し、公開用のデータフロー ID とアクション操作を指定します。

**API 形式**

```http
POST /flows/{FLOW_ID}/action?op=publish
```

| パラメーター | 説明 |
| --- | --- |
| `{FLOW_ID}` | 公開するデータフローの ID。 |
| `op` | クエリされたデータフローの状態を更新するアクション操作。ドラフトデータフローを公開するには、`op` を `publish` に設定します。 |

+++リクエスト

次のリクエストでは、ドラフトデータフローを公開しています。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows/a7e88a01-40f9-4ebf-80b2-0fc838ff82ef/action?op=publish' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

+++応答

正常な応答は、データフローの ID と対応する `etag` を返します。

```json
{
  "id": "a7e88a01-40f9-4ebf-80b2-0fc838ff82ef",
  "etag": "\"4b0354b7-0000-0200-0000-6716ce1f0000\""
}
```

+++

Experience Platform UI を使用すると、ドラフトデータフローが公開されたことを確認できます。 ソースカタログのデータフローページに移動し、データフローの **[!UICONTROL ステータス]** を参照します。 成功した場合、ステータスは **有効** に設定されます。

>[!TIP]
>
>* フィルタリングが有効になっているデータフローは、1 回だけバックフィルされます。 フィルタリング条件（追加または削除）でおこなった変更は、増分データに対してのみ有効にできます。
>* 新しいアクティビティタイプの履歴データを取り込む必要がある場合は、新しいデータフローを作成し、フィルター条件の適切なアクティビティタイプを使用してフィルター条件を定義することをお勧めします。
>* カスタムアクティビティタイプはフィルタリングできません。
>* フィルターされたデータをプレビューできません。

## 付録

この節では、フィルタリングに使用する様々なペイロードの例をさらに示します。

### 特異条件

1 つの条件のみを必要とするシナリオでは、初期 `fnApply` を省略できます。

+++選択すると例が表示されます

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

+++

### `in` 演算子の使用

演算子 `in` の例については、以下のサンプルペイロードを参照してください。

+++選択すると例が表示されます

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

+++

### `isNull` 演算子の使用

+++選択すると例が表示されます

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

+++

### `NOT` 演算子の使用

演算子 `NOT` の例については、以下のサンプルペイロードを参照してください。


+++選択すると例が表示されます

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

+++

### ネストされた条件を使用した例

複雑なネスト条件の例については、以下のサンプルペイロードを参照してください。

+++選択すると例が表示されます

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

+++