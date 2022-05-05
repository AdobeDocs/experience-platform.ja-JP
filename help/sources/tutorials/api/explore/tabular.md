---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；API；参照；フローサービス
title: フローサービス API を使用した表形式のソースの調査
description: このチュートリアルでは、フローサービス API を使用して、テーブルベースのソースのコンテンツと構造を調べます。
source-git-commit: 1333eac5e022ef32f051120496154a88e2f9324e
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 23%

---

# を使用してデータテーブルを調査する [!DNL Flow Service] API

このチュートリアルでは、 [[!DNL Flow Service]](https://www.adobe.io/experience-platform-apis/references/flow-service/) API

>[!NOTE]
>
> データテーブルを調べるには、表形式のソースに対して有効なベース接続 ID が既に存在している必要があります。 この ID がない場合は、次のチュートリアルを参照して、表形式のソースのベース接続 ID を作成する手順を確認してください。 <ul><li>[広告](../../../home.md#advertising)</li><li>[CRM](../../../home.md#customer-relationship-management)</li><li>[カスタマーサクセス](../../../home.md#customer-success)</li><li>[データベース](../../../home.md#database)</li><li>[E コマース](../../../home.md#ecommerce)</li><li>[マーケティングの自動処理](../../../home.md#marketing-automation)</li><li>[支払い](../../../home.md#payments)</li><li>[プロトコル](../../../home.md#protocols)</li></ul>

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

## データテーブルの調査

GETリクエストを [!DNL Flow Service] ソースのベース接続 ID を提供する際の API。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ソースのベース接続 ID。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/5e73e5a2-dc36-45a8-9f16-93c7a43af318/explore?objectType=root' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、ソースからテーブルの配列を返します。 Platform に取り込むテーブルを見つけて、そのテーブルをメモします。 `path` プロパティに含める必要がある場合は、次の手順で指定して、その構造を調べます。

```json
[
    {
        "type": "table",
        "name": "ACME Spring Campaign",
        "path": "acmeSpringCampaign",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "ACME Summer Campaign",
        "path": "acmeSummerCampaign",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## Inspectテーブルの構造

データテーブルの内容を調べるには、に対してGETリクエストを実行します。 [!DNL Flow Service] テーブルのパスをクエリパラメーターとして指定する際の API。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ソースのベース接続 ID。 |
| `{TABLE_PATH}` | 検査するテーブルの path プロパティです。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/5e73e5a2-dc36-45a8-9f16-93c7a43af318/explore?objectType=table&object=acmeSpringCampaign' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、指定したテーブルの内容と構造に関する情報が返されます。 各テーブルの列に関する詳細は、 `columns` 配列。

```json
{
  "format": "flat",
  "schema": {
    "columns": [
      {
        "name": "TestID",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "Name",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "Datefield",
        "type": "string",
        "meta:xdmType": "date-time",
        "xdm": {
          "type": "string",
          "format": "date-time"
        }
      },
      {
        "name": "complaint_type",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "complaint_description",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "status",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "status_change_date",
        "type": "string",
        "meta:xdmType": "date-time",
        "xdm": {
          "type": "string",
          "format": "date-time"
        }
      },
      {
        "name": "city",
        "type": "string",
        "xdm": {
          "type": "string"
        }
      },
      {
        "name": "Datefield2",
        "type": "string",
        "meta:xdmType": "date-time",
        "xdm": {
          "type": "string",
          "format": "date-time"
        }
      }
    ]
  }
}
```

## 次の手順

このチュートリアルでは、データテーブルの構造と内容に関する情報を収集しました。 さらに、Platform に取り込むテーブルへのパスを取得しました。 この情報を使用して、ソース接続とデータフローを作成し、データを Platform に取り込むことができます。 を使用してソース接続とデータフローを作成する手順については、次のチュートリアルを参照してください [!DNL Flow Service] API:

* [広告ソース](../collect/advertising.md)
* [CRM ソース](../collect/crm.md)
* [顧客成功ソース](../collect/customer-success.md)
* [データベースソース](../collect/database-nosql.md)
* [E コマースソース](../collect/ecommerce.md)
* [マーケティング自動化ソース](../collect/marketing-automation.md)
* [支払いソース](../collect/payments.md)
* [プロトコルソース](../collect/protocols.md)