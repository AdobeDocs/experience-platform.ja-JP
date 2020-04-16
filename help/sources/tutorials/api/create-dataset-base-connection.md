---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用したExperience Platformデータセットベースの接続の作成
topic: overview
translation-type: tm+mt
source-git-commit: e409b287d6965ede4030829287bd3e405e9d709b

---


# Flow Service APIを使用したExperience Platformデータセットベースの接続の作成

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

サードパーティのソースのデータをプラットフォームに接続するには、まずデータセットベースの接続を確立する必要があります。

このチュートリアルでは、フローサービスAPIを使用して、データセットベースの接続を作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマレジストリ開発者ガイド](../../../xdm/api/getting-started.md):スキーマレジストリAPIの呼び出しを正常に実行するために知っておく必要がある重要な情報が含まれます。 これには、ユーザ `{TENANT_ID}`ー、「コンテナ」の概念、リクエストを行うために必要なヘッダー（Acceptヘッダーとその可能な値に特に注意）が含まれます。
* [Catalog Service](../../../catalog/home.md):カタログは、エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。
* [バッチインジェスト](../../../ingestion/batch-ingestion/overview.md):バッチ取り込みAPIを使用すると、データをバッチファイルとしてエクスペリエンスプラットフォームに取り込むことができます。
* [サンドボックス](../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してData Lakeに正常に接続するために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## 接続仕様の検索

データセットベース接続を作成する最初の手順は、フローサービス内から一連の接続仕様を取得することです。

**API形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、接続パラメーターを使用して、データセットベースの接続の接続仕様を調べることができます。クエリ

GETリクエストをクエリパラメータなしで送信すると、使用可能なすべてのソースの接続指定が返されます。 データセットベース接続のクエリ `property=id=="c604ff05-7f1a-43c0-8e18-33bf874cb11c"` を含めて、データセットベース接続の情報を取得できます。

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

成功した応答は、ベース接続の作成に必要な接続指定と一意の識別子(`id`)を返します。

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

## データセットベース接続の作成

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクターを作成する場合に使用できるデータセットベース接続は1つだけです。

**API形式**

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
| `connectionSpec.id` | 前の手順で取得 `id` した接続仕様です。 |

**応答**

成功した応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、サードパーティのターゲット接続を作成し、サードパーティのソースコネクタからデータを取り込むために必要です。

```json
{
    "id": "d6c3988d-14ef-4000-8398-8d14ef000021",
    "etag": "\"d502e61b-0000-0200-0000-5e62a1f90000\""
}
```

## 次の手順

このチュートリアルに従うと、フローサービスAPIを使用してデータセットベースの接続を作成し、接続の一意のID値を取得できます。 この基本接続を使用して、接続を作成できます。ターゲット接続 次のチュートリアルでは、使用するソースコネクタのカテゴリに応じて、ターゲット接続を作成する手順を説明します。

* [クラウドストレージ](./collect/cloud-storage.md)
* [CRM](./collect/crm.md)
* [顧客の成功](./collect/customer-success.md)
* [データベース](./collect/database-nosql.md)