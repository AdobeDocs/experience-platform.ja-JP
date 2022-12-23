---
title: 高速クエリエンドポイント
description: クエリ高速化ストアにステートレスでアクセスし、集計データに基づいて結果をすばやく返す方法を説明します。このドキュメントでは、クエリサービス高速クエリエンドポイントに対する HTTP リクエストと応答のサンプルを示します。
exl-id: 29ea4d25-9c46-4b29-a6d7-45ac33dcb0fb
source-git-commit: a9887535b12b8c4aeb39bb5a6646da88db4f0308
workflow-type: ht
source-wordcount: '567'
ht-degree: 100%

---

# 高速クエリエンドポイント

Data Distiller SKU の一部である [Query Service API](https://developer.adobe.com/experience-platform-apis/references/query-service/) では、高速ストアに対してステートレスにクエリを実行できます。返される結果は、集計データに基づいています。結果の待ち時間が短縮され、情報をよりインタラクティブに交換できるようになります。また、高速クエリ API は[ユーザー定義ダッシュボード](../../dashboards/user-defined-dashboards.md)の強化にも使われます。

このガイドを進める前に、Query Service API を正しく使用するために[Query Service API ガイド](./getting-started.md)を読み、内容を理解していることを確認してください。

## はじめに

Data Distiller SKU は、クエリ高速化ストアを使用する場合に必要です。Data Distiller SKU に関連する[パッケージ](../packages.md)、[ガードレール](../guardrails.md#query-accelerated-store)および[ライセンス](../data-distiller/licence-usage.md)のドキュメントを参照してください。Data Distiller SKU をお持ちでない場合は、アドビのカスタマーサービス担当者に詳細をお問い合わせください。

以下の節では、 Query Service API を使用して、ステートレスな方法でクエリ高速ストアにアクセスするために必要な API 呼び出しについて説明します。各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

## 高速クエリの実行 {#run-accelerated-query}

`/accelerated-queries` エンドポイントへの POST リクエストを使用して高速クエリを実行します。クエリは、リクエストペイロードに直接含まれるか、テンプレート ID を使用して参照されます。

**API 形式**

```shell
POST /accelerated-queries
```

### リクエスト

>[!IMPORTANT]
>
>`/accelerated-queries` エンドポイントへのリクエストには SQL 文かテンプレート ID のどちらか一方が必要ですが、両方は不要です。リクエストで両方を送信すると、エラーが発生します。

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

この代替リクエストでは、リクエスト本文のテンプレート ID を高速ストアに送信します。対応するテンプレートの SQL を使用して、高速ストアに対するクエリを実行します。

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
| `dbName` | 高速クエリを実行するデータベースの名前。`dbName` の値は `{SANDBOX_NAME}:{ACCELERATED_STORE_DATABASE}.{ACCELERATED_STORE_SCHEMA}` の形式を取る必要があります。指定されたデータベースは、高速ストア内に存在する必要があります。存在しない場合、リクエストがエラーになります。また、`x-sandbox-name` のヘッダーと `dbName` のサンドボックス名が同じサンドボックスを参照していることを確認する必要があります。 |
| `sql` | SQL 文の文字列。最大サイズは 1000000 文字です。 |
| `templateId` | `/templates` エンドポイントに対して POST リクエストが行なわれた際に、テンプレートとして作成および保存されたクエリの一意の識別子。 |
| `name` | 人間にとってわかりやすい高速クエリの名前（オプション）。 |
| `description` | 他のユーザーが目的を理解するのに役立つ、クエリの目的に関するコメント（オプション）。最大サイズは 1000 バイトです。 |

**応答**

応答が成功すると、HTTP ステータス 200 と、クエリによって作成されたアドホックスキーマが返されます。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられています。

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
| `resultsMeta._adhoc` | 単一のデータセットでのみ使用するために名前空間が使用されたフィールドを持つアドホックの Experience Data Model（XDM）スキーマ。 |
| `resultsMeta._adhoc.type` | アドホックスキーマのデータタイプ。 |
| `resultsMeta._adhoc.meta:xdmType` | これは、XDM フィールドタイプに対してシステムで生成される値です。利用可能なタイプについて詳しくは、[利用可能な XDM タイプ](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/custom-fields-api.html?lang=ja)のドキュメントを参照してください。 |
| `resultsMeta._adhoc.properties` | クエリされたデータセットの列名です。 |
| `resultsMeta._adhoc.results` | これらは、クエリされたデータセットの行名です。返される各列を反映します。 |
