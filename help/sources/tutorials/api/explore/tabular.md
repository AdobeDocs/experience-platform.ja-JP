---
keywords: Experience Platform；ホーム；人気のトピック；ソース；API；参照；フローサービス
title: Flow Service API を使用した表形式のSourceの調査
description: このチュートリアルでは、Flow Service API を使用して、テーブルベースのソースの内容と構造を調べます。
exl-id: 0c7a5b8a-2071-4ac2-b2d1-c5534e7c7d9c
source-git-commit: 3bdeec8284873b8d9368f833b24e9922ed489019
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 21%

---

# [!DNL Flow Service] API を使用したデータテーブルの調査

このチュートリアルでは、[[!DNL Flow Service]](https://www.adobe.io/experience-platform-apis/references/flow-service/) API を使用してデータテーブルの構造と内容を調査およびプレビューする手順を説明します。

>[!NOTE]
>
> データテーブルを調べるには、表形式のソースに対して有効なベース接続 ID が必要です。 この ID を持っていない場合は、次のチュートリアルで、表形式のソースのベース接続 ID を作成する手順を確認してください。 <ul><li>[広告](../../../home.md#advertising)</li><li>[CRM](../../../home.md#customer-relationship-management)</li><li>[ カスタマーサクセス ](../../../home.md#customer-success)</li><li>[データベース](../../../home.md#database)</li><li>[E コマース ](../../../home.md#ecommerce)</li><li>[ マーケティングの自動処理 ](../../../home.md#marketing-automation)</li><li>[ 支給 ](../../../home.md#payments)</li><li>[ プロトコル ](../../../home.md#protocols)</li></ul>

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

## データテーブルの探索

ソースのベース接続 ID を指定したうえで [!DNL Flow Service] API に対してデータリクエストを行うことで、GETテーブルの構造に関する情報を取得できます。

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

応答が成功すると、ソースからテーブルの配列が返されます。 Platform に取り込むテーブルを見つけ、その `path` プロパティをメモします。これは、その構造を検査するために次の手順で指定する必要があるからです。

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

## テーブルの構造のInspect

データテーブルの内容を調べるには、テーブルのパスをクエリパラメーターとして指定して、[!DNL Flow Service] API にGETリクエストを実行します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ソースのベース接続 ID。 |
| `{TABLE_PATH}` | 検査するテーブルのパスプロパティ。 |

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

応答が成功すると、指定されたテーブルのコンテンツと構造に関する情報が返されます。 テーブルの各列に関する詳細は、`columns` 配列の要素内にあります。

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

このチュートリアルでは、データテーブルの構造と内容に関する情報を収集しました。 さらに、Platform に取り込むテーブルへのパスを取得しました。 この情報を使用して、ソース接続とデータフローを作成し、データを Platform に取り込むことができます。 [!DNL Flow Service] API を使用してソース接続とデータフローを作成する方法に関する具体的な手順については、次のチュートリアルを参照してください。

* [Advertising ソース](../collect/advertising.md)
* [CRM ソース](../collect/crm.md)
* [カスタマーサクセスソース](../collect/customer-success.md)
* [データベースソース](../collect/database-nosql.md)
* [E コマースソース](../collect/ecommerce.md)
* [マーケティング自動化ソース](../collect/marketing-automation.md)
* [支払いソース](../collect/payments.md)
* [プロトコルソース](../collect/protocols.md)
