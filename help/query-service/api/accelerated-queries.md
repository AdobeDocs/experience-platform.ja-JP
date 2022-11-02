---
title: Accelerated Queries Endpoint
description: クエリ Accelerated Store にステートレスでアクセスし、集計データに基づいて結果をすばやく返す方法を説明します。 このドキュメントでは、Query Service accelerated-queries エンドポイントに対する HTTP リクエストと応答のサンプルを示します。
source-git-commit: 2a9d40fc783feb78a1d5ad7eb615ceb40097eb89
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 11%

---

# 高速クエリエンドポイント

Data Distiller SKU の一部として、 [クエリサービス API](https://developer.adobe.com/experience-platform-apis/references/query-service/) では、accelerated store に対してステートレスクエリを実行できます。 返される結果は、集計データに基づいています。 結果の待ち時間が短縮され、情報をよりインタラクティブに交換できるようになります。 高速クエリ API は、 [ユーザー定義ダッシュボード](../../dashboards/user-defined-dashboards.md).

このガイドを進める前に、 [クエリサービス API ガイド](./getting-started.md) クエリサービス API を正しく使用するために。

## はじめに

Data Distiller SKU は、クエリアクセラレーションストアを使用する場合に必要です。 Data Distiller SKU に関連する[パッケージ](../packages.md)、[ガードレール](../guardrails.md#query-accelerated-store)および[ライセンス](../data-distiller/licence-usage.md)のドキュメントを参照してください。Data Distiller SKU をお持ちでない場合は、アドビのカスタマーサービス担当者に詳細をお問い合わせください。

以下の節では、クエリサービス API を使用して、ステートレスな方法でクエリ高速化ストアにアクセスするために必要な API 呼び出しについて説明します。 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

## 高速クエリの実行 {#run-accelerated-query}

へのPOSTリクエスト `/accelerated-queries` endpoint を使用して高速クエリを実行します。 クエリは、リクエストペイロードに直接含まれるか、テンプレート ID を使用して参照されます。

**API 形式**

```shell
POST /accelerated-queries
```

### リクエスト

>[!IMPORTANT]
>
>へのリクエスト `/accelerated-queries` エンドポイントには SQL ステートメント ID かテンプレート ID のどちらか一方が必要ですが、両方は不要です。 リクエストで両方を送信すると、エラーが発生します。

次のリクエストでは、リクエスト本文の SQL クエリを高速ストアに送信します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/acceleated-queries
 -H 'Authorization: {ACCESS_TOKEN}'
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'Content-Type: application/json' \
 -H 'Accept: application/json' \
 -d '
 {
   "dbName": "acmesbox1:acmeacceldb:accmeaggschema",
   "sql": "SELECT * FROM accounts;",
   "name": "Sample Accelerated Query",
   "description": "A sample of an accelerated query."
 }
'
```

この代替リクエストでは、リクエスト本文のテンプレート ID を高速ストアに送信します。 対応するテンプレートの SQL を使用して、高速ストアに対するクエリを実行します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/acceleated-queries
 -H 'Authorization: {ACCESS_TOKEN}'
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'Content-Type: application/json' \
 -H 'Accept: application/json' \
 -d '
 {
   "dbName": "acmesbox1:acmeacceldb:accmeaggschema",
   "templateId": "5d8228e7-4200-e3de-11e9-7f27416c5f0d",
   "name": "Sample Accelerated Query",
   "description": "A sample of an accelerated query."
 }
'
```

| プロパティ | 説明 |
|---|---|
| `dbName` | 高速クエリを実行するデータベースの名前。 の値 `dbName` ～の形式を取るべきである `{SANDBOX_NAME}:{ACCELERATED_STORE_DATABASE}.{ACCELERATED_STORE_SCHEMA}`. 指定されたデータベースは、高速ストア内に存在する必要があります。存在しない場合、リクエストがエラーになります。 また、 `x-sandbox-name` のヘッダーとサンドボックス名 `dbName` 同じサンドボックスを参照してください。 |
| `sql` | SQL 文の文字列。 最大サイズは1000000文字です。 |
| `templateId` | テンプレートリクエストがに対しておこなわれる際に、テンプレートとして作成および保存されたPOSTの一意の識別子 `/templates` endpoint. |
| `name` | 高速クエリのわかりやすくわかりやすい名前（オプション）。 |
| `description` | 他のユーザーが目的を理解するのに役立つ、クエリの目的に関するオプションのコメント。 最大サイズは 1000 バイトです。 |

**応答**

正常な応答は、HTTP ステータス 200 と、クエリによって作成されたアドホックスキーマを返します。

>[!NOTE]
>
>次の応答は、簡潔にするために切り捨てられました。

```json
{
  "queryId": "315a0a66-0fbb-4810-bc30-484cea5e0f1e",
  "resultsMeta": {
    "_adhoc": {
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
                "Units": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Industry_code_NZSIOC": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Industry_name_NZSIOC": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Variable_code": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Variable_name": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Industry_aggregation_NZSIOC": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Value": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Year": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Variable_category": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                },
                "Industry_code_ANZSIC06": {
                    "type": "string",
                    "meta:xdmType": "string",
                    "default": null
                }
            }
        }
    },
  "results": [
     {
            "Units": "Dollars (millions)",
            "Industry_code_NZSIOC": "CC411",
            "Industry_name_NZSIOC": "Printing",
            "Variable_code": "H26",
            "Variable_name": "Fixed tangible assets",
            "Industry_aggregation_NZSIOC": "Level 4",
            "Value": "282",
            "Year": "2020",
            "Variable_category": "Financial position",
            "Industry_code_ANZSIC06": "ANZSIC06 groups C161 and C162"
        },
        {
            "Units": "Dollars (millions)",
            "Industry_code_NZSIOC": "CC411",
            "Industry_name_NZSIOC": "Printing",
            "Variable_code": "H27",
            "Variable_name": "Additions to fixed assets",
            "Industry_aggregation_NZSIOC": "Level 4",
            "Value": "35",
            "Year": "2020",
            "Variable_category": "Financial position",
            "Industry_code_ANZSIC06": "ANZSIC06 groups C161 and C162"
        },
        {
            "Units": "Dollars (millions)",
            "Industry_code_NZSIOC": "CC411",
            "Industry_name_NZSIOC": "Printing",
            "Variable_code": "H28",
            "Variable_name": "Disposals of fixed assets",
            "Industry_aggregation_NZSIOC": "Level 4",
            "Value": "9",
            "Year": "2020",
            "Variable_category": "Financial position",
            "Industry_code_ANZSIC06": "ANZSIC06 groups C161 and C162"
        },
        ...
    ],
  "request": {
    "dbName": "acmesbox1:acmeacceldb:accmeaggschema",
    "sql": "SELECT * FROM accounts;",
    "name": "Sample Accelerated Query",
    "description": "A sample of an accelerated query."
  }
}
```

| プロパティ | 説明 |
|---|---|
| `queryId` | 作成したクエリの ID 値。 |
| `resultsMeta` | このオブジェクトには、結果で返される各列のメタデータが含まれるので、各列の名前とタイプがユーザーにわかります。 |
| `resultsMeta._adhoc` | 単一のデータセットでのみ使用するために名前空間が使用されたフィールドを持つアドホック Experience Data Model(XDM) スキーマ。 |
| `resultsMeta._adhoc.type` | アドホックスキーマのデータタイプ。 |
| `resultsMeta._adhoc.meta:xdmType` | これは、XDM フィールドタイプに対してシステムで生成される値です。 利用可能なタイプについて詳しくは、 [使用可能な XDM タイプ](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/custom-fields-api.html). |
| `resultsMeta._adhoc.properties` | クエリされたデータセットの列名です。 |
| `resultsMeta._adhoc.results` | これらは、クエリされたデータセットの行名です。 返される各列を反映します。 |

