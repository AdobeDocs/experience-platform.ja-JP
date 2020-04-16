---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用してAzure BLOBコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 5c9bc1ec9170e4971a7d693038d12315aca616d5

---


# フローサービスAPIを使用してAzure BLOBコネクタを作成する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、フローサービスAPIを使用して、Experience PlatformをAzure BLOB（以下「BLOB」と呼ばれる）ストレージに接続する手順を説明します。

エクスペリエンスプラットフォームでユーザーインターフェイスを使用したい場合は、 [Azure BlobまたはAmazon S3ソースコネクタのUIチュートリアルで](../../../ui/create/cloud-storage/blob-s3.md) 、同様の操作を実行するための手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してBLOBストレージに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

フローサービスがBLOBストレージと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | Blobストレージ内のデータにアクセスするために必要な接続文字列。 |

開始方法の詳細については、このAzure BLOB [ドキュメント](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## 接続仕様の検索

プラットフォームをBLOBストレージに接続する前に、BLOBの接続仕様が存在することを確認する必要があります。 接続仕様が存在しない場合は、接続を確立できません。

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、接続パラメータを使用して、Blobの接続指定をクエリできます。

**API形式**

GETリクエストをクエリパラメータなしで送信すると、使用可能なすべてのソースの接続指定が返されます。 この情報を含めて、Blob専用のクエリ `property=name=="azure-blob"` を取得することができます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="azure-blob"
```

**リクエスト**

次の要求は、Blobの接続指定を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="azure-blob"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、一意の識別子(`id`)を含むBlobの接続指定を返します。 このIDは、次の手順でベース接続を作成する際に必要です。

```json
{
    "items": [
        {
            "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
            "name": "azure-blob",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "ConnectionString",
                    "type": "ConnectionString",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "properties": {
                            "connectionString": {
                                "type": "string",
                                "format": "password"
                            }
                        },
                        "required": [
                            "connectionString"
                        ]
                    }
                }
            ]
        }
    ]
}
```

## ベース接続の作成

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できるので、Blobアカウントごとに1つのベース接続が必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Blob Base Connection",
        "description": "Base connection for an Azure Blob account",
        "auth": {
            "specName": "ConnectionString",
            "params": {
                "connectionString": "{CONNECTION_STRING}"
            }
        },
        "connectionSpec": {
            "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | Blobストレージの接続文字列。 |
| `connectionSpec.id` | 前の手順で取得 `id` したBlobストレージの接続指定。 |

**応答**

成功した応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでストレージを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用してBlob接続を作成し、一意のIDを応答本文の一部として取得できます。 この接続IDを使用して、Flow Service APIを使用してク [ラウドストレージを調べたり、Flow Service APIを使用して](../../explore/cloud-storage.md) Parket [データを取り込んだりすることができます](../../cloud-storage-parquet.md)。