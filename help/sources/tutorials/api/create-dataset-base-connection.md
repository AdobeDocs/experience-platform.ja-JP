---
keywords: Experience Platform;home;popular topics;dataset connection flow service;flow service;Flow service connection
solution: Experience Platform
title: Flow Service APIを使用してExperience Platformデータセットベースの接続を作成する
topic: overview
type: Tutorial
description: フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元管理するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 23%

---


# APIを使用した [!DNL Experience Platform][!DNL Flow Service] データセットベースの接続の作成

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

サードパーティのソースのデータをソースに接続するに [!DNL Platform]は、まずデータセットベースの接続を確立する必要があります。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、データセットベースの接続を作成する手順を順を追って説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマレジストリ開発ガイド](../../../xdm/api/getting-started.md):スキーマレジストリAPIの呼び出しを正常に実行するために知っておく必要がある重要な情報が含まれます。 これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
* [カタログサービス](../../../catalog/home.md):カタログは、内のデータの場所と系列のレコードシステムで [!DNL Experience Platform]す。
* [バッチインジェスト](../../../ingestion/batch-ingestion/overview.md):バッチ取り込みAPIを使用すると、データをバッチファイルとしてExperience Platformに取り込むことができます。
* [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to Data Lake using the [!DNL Flow Service] API.

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続仕様の検索

データセットベースの接続を作成する最初の手順は、内から一連の接続仕様を取得すること [!DNL Flow Service]です。

**API 形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、クエリパラメーターを使用して、データセットベースの接続の接続仕様を調べることができます。

クエリパラメーターを指定しないでGETリクエストを送信すると、使用可能なすべてのソースの接続仕様が返されます。 クエリを組み込むと、データセットベース `property=id=="c604ff05-7f1a-43c0-8e18-33bf874cb11c"` の接続に関する情報を取得できます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=id=="c604ff05-7f1a-43c0-8e18-33bf874cb11c"
```

**リクエスト**

次のリクエストは、データセットベース接続の接続仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=id=="c604ff05-7f1a-43c0-8e18-33bf874cb11c"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、接続仕様と、ベース接続の作成に必要な固有識別子(`id`)を返します。

```json
{
    "items": [
        {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "name": "{NAME}",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "targetSpec": {
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "dataSetId": {
                            "type": "string"
                        }
                    },
                    "required": [
                        "dataSetId"
                    ]
                }
            },
            "attributes": {
                "category": "{CATEGORY}"
            },
            "permissionsInfo": {
                "view": [
                    {
                        "@type": "lowLevel",
                        "name": "Dataset",
                        "permissions": [
                            "read"
                        ]
                    }
                ],
                "manage": [
                    {
                        "@type": "lowLevel",
                        "name": "Dataset",
                        "permissions": [
                            "write"
                        ]
                    }
                ]
            }
        }
    ]
}
```

## データセットベースの接続の作成

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクターを作成する場合に使用できるので、必要なデータセットベース接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Dataset Base Connection",
        "description": "Dataset Base Connection",
        "connectionSpec": {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `connectionSpec.id` | 前の手順で取得 `id` した接続仕様。 |

**応答** 

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、ターゲット接続を作成し、サードパーティのソースコネクタからデータを取り込むために必要です。

```json
{
    "id": "d6c3988d-14ef-4000-8398-8d14ef000021",
    "etag": "\"d502e61b-0000-0200-0000-5e62a1f90000\""
}
```

## 次の手順

このチュートリアルに従うと、 [!DNL Flow Service] APIを使用したデータセットベースの接続を作成し、接続の一意のID値を取得したことになります。 この基本接続を使用して、ターゲット接続を作成できます。 次のチュートリアルでは、使用するソースコネクタのカテゴリに応じて、ターゲット接続を作成する手順を説明します。

* [クラウドストレージ](./collect/cloud-storage.md)
* [CRM](./collect/crm.md)
* [顧客の成功](./collect/customer-success.md)
* [データベース](./collect/database-nosql.md)