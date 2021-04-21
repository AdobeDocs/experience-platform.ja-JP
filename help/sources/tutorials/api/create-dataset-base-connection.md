---
keywords: Experience Platform；ホーム；人気の高いトピック；データセット接続フローサービス；フローサービス；フローサービス接続
solution: Experience Platform
title: Flow Service APIを使用したAdobe Experience Platformデータセットベース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience Platformへのデータセットベース接続を作成する方法を説明します。
exl-id: 5e829f4a-954b-4011-a003-c42c7a0d5617
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '721'
ht-degree: 28%

---

# [!DNL Flow Service] APIを使用した[!DNL Experience Platform]データセットベースの接続の作成

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

サードパーティのソースのデータを[!DNL Platform]に接続するには、まずデータセットベースの接続を確立する必要があります。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、データセットベースの接続を作成する手順を順を追って説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマレジストリ開発ガイド](../../../xdm/api/getting-started.md):スキーマレジストリAPIの呼び出しを正常に実行するために知っておく必要がある重要な情報が含まれます。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
* [カタログサービス](../../../catalog/home.md):カタログは、内のデータの場所と系列のレコードシステムで [!DNL Experience Platform]す。
* [バッチインジェスト](../../../ingestion/batch-ingestion/overview.md):バッチ取り込みAPIを使用すると、データをバッチファイルとしてExperience Platformに取り込むことができます。
* [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してData Lakeに正常に接続するために必要な追加情報については、以下の節で説明します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続仕様の検索

データセットベースの接続を作成する最初の手順は、[!DNL Flow Service]内から一連の接続仕様を取得することです。

**API 形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、クエリパラメーターを使用して、データセットベースの接続の接続仕様を調べることができます。

クエリパラメーターを指定しないでGETリクエストを送信すると、使用可能なすべてのソースの接続仕様が返されます。 クエリ`property=id=="c604ff05-7f1a-43c0-8e18-33bf874cb11c"`を含めて、データセットベースの接続に関する情報を取得できます。

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

正常な応答は、接続仕様と、ベース接続の作成に必要な一意の識別子(`id`)を返します。

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
| `connectionSpec.id` | 前の手順で取得した接続仕様`id`。 |

**応答**

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を返します。 このIDは、ターゲット接続を作成し、サードパーティのソースコネクタからデータを取り込むために必要です。

```json
{
    "id": "d6c3988d-14ef-4000-8398-8d14ef000021",
    "etag": "\"d502e61b-0000-0200-0000-5e62a1f90000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用したデータセットベースの接続を作成し、接続の固有のID値を取得したことになります。 この基本接続を使用して、ターゲット接続を作成できます。 次のチュートリアルでは、使用するソースコネクタのカテゴリに応じて、ターゲット接続を作成する手順を説明します。

* [クラウドストレージ](./collect/cloud-storage.md)
* [CRM](./collect/crm.md)
* [顧客の成功](./collect/customer-success.md)
* [データベース](./collect/database-nosql.md)
