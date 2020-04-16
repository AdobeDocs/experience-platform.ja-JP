---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用してGoogle Cloudストレージコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 96be7084d5d2efb86b7bba27f1a92bd9c0055fba

---


# フローサービスAPIを使用してGoogle Cloudストレージコネクタを作成する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、フローサービスAPIを使用して、Experience PlatformをGoogle Cloudストレージアカウントに接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してGoogle Cloudストレージアカウントに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

フローサービスがGoogle Cloudストレージアカウントに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | Google CloudストレージアカウントのアクセスキーID。 |
| `secretAccessKey` | Google Cloudストレージアカウントの秘密アクセスキー。 |

使い始める前に、このGoogle Cloud [ドキュメント](https://cloud.google.com/docs/authentication)。

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

PlatformをGoogle Cloudストレージに接続する前に、Google Cloudストレージの接続仕様が存在することを確認する必要があります。 接続仕様が存在しない場合は、接続を確立できません。

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、ストレージパラメーターを使用して、Google Cloudクエリの接続指定を検索できます。

**API形式**

GETリクエストをクエリパラメータなしで送信すると、使用可能なすべてのソースの接続指定が返されます。 このクエリを含めて、Google Cloud `property=name=="google-cloud"` ストレージ専用の情報を取得できます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name==google-cloud
```

**リクエスト**

次のリクエストは、Google Cloudストレージの接続仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="google-cloud"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した場合、Google Cloudストレージの固有な識別子(`id`)を含む接続仕様が返されます。 このIDは、次の手順でベース接続を作成する際に必要です。

```json
{
    "items": [
        {
            "id": "32e8f412-cdf7-464c-9885-78184cb113fd",
            "name": "google-cloud",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication for google-cloud",
                    "type": "Basic Authentication",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params required for connecting to google-cloud storage connector.",
                        "properties": {
                            "accessKeyId": {
                                "type": "string",
                                "description": "Access Key Id for the user account"
                            },
                            "secretAccessKey": {
                                "type": "string",
                                "description": "Secret Access Key for the user account",
                                "format": "password"
                            }
                        },
                        "required": [
                            "accessKeyId",
                            "secretAccessKey"
                        ]
                    }
                }
            ]
        }
    ]
}
```

## ベース接続の作成

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるので、Google Cloudストレージアカウントごとに1つのベース接続が必要です。

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
        "name": "Google Cloud Storage base connection",
        "description": "Base connector for Google Cloud Storage",
        "auth": {
            "specName": "Basic Authentication for google-cloud",
            "params": {
                "accessKeyId": "accessKeyId",
                "secretAccessKey": "secretAccessKey"
            }
        },
        "connectionSpec": {
            "id": "32e8f412-cdf7-464c-9885-78184cb113fd",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.accessKeyId` | Google Cloudストレージアカウントに関連付けられているアクセスキーID。 |
| `auth.params.secretAccessKey` | Google Cloudストレージアカウントに関連付けられている秘密アクセスキー。 |
| `connectionSpec.id` | 前の手順で取得 `id` したGoogle Cloudストレージアカウントの接続指定。 |

**応答**

成功した応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用してGoogle Cloudストレージ接続を作成し、一意のIDを応答本文の一部として取得できます。 この接続IDを使用して、Flow Service APIを使用してク [ラウドストレージを調べたり、Flow Service APIを使用して](../../explore/cloud-storage.md) Parket [データを取り込んだりすることができます](../../cloud-storage-parquet.md)。