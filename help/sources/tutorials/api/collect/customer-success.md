---
keywords: Experience Platform;home;popular topics;collect customer success;customer success
solution: Experience Platform
title: ソースコネクターとAPIを使用して顧客成功システムからデータを収集する
topic: overview
type: Tutorial
description: このチュートリアルでは、顧客の成功システムからデータを取得し、ソースコネクタとAPIを使用してプラットフォームに取り込む手順を説明します。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 13%

---


# ソースコネクターとAPIを使用して顧客成功システムからデータを収集する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、サードパーティの顧客成功システムからデータを取得し、ソースコネクタと [!DNL Platform][[!DNL Flow Service]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml) APIを使用してデータを取り込む手順について説明します。

## はじめに

このチュートリアルでは、有効な接続と、ファイルのパスや構造など、使用するファイルに関する情報を通じて、サードパーティの顧客成功システムにアクセスでき [!DNL Platform]る必要があります。 この情報がない場合は、このチュートリアルを試みる前に、Flow Service APIを使用したデータベースまたはNoSQLシステムの [詳細に関するチュートリアルを参照してください](../explore/customer-success.md) 。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマレジストリ開発ガイド](../../../../xdm/api/getting-started.md):スキーマレジストリAPIの呼び出しを正常に実行するために知っておく必要がある重要な情報が含まれます。 これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
* [[!DNL Catalog Service]](../../../../catalog/home.md):カタログは、内のデータの場所と系列のレコードシステムで [!DNL Experience Platform]す。
* [[!DNL Batch ingestion]](../../../../ingestion/batch-ingestion/overview.md):Batch Ingestion APIを使用すると、データをバッチファイル [!DNL Experience Platform] としてに取り込むことができます。
* [サンドボックス](../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to a customer success system using the [!DNL Flow Service] API.

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## ソース接続の作成 {#source}

You can create a source connection by making a POST request to the [!DNL Flow Service] API. ソース接続は、接続ID、ソースデータファイルのパス、および接続仕様IDで構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

ファイルベースのコネクタの列挙値は、次のとおりです。

| Data.format | 列挙値 |
| ----------- | ---------- |
| 区切りファイル | `delimited` |
| JSONファイル | `json` |
| パーケファイル | `parquet` |

すべてのテーブルベースのコネクタで、列挙値を使用します。 `tabular`.

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Source connection for Customer Success",
        "baseConnectionId": "f1da3694-38a9-403d-9a36-9438a9203d42",
        "description": "Source connection for a Customer Success connector",
        "data": {
            "format": "tabular",
        },
        "params": {
            "path": "Account"
        },
        "connectionSpec": {
            "id": "cb66ab34-8619-49cb-96d1-39b37ede86ea",
            "version": "1.0"
        }
    }}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `baseConnectionId` | アクセスするサードパーティ顧客成功システムの一意の接続ID。 |
| `params.path` | ソースファイルのパス。 |
| `connectionSpec.id` | 特定のサードパーティ顧客成功システムに関連付けられた接続仕様ID。 接続仕様IDのリストについては、 [付録](#appendix) を参照してください。 |

**応答** 

正常な応答は、新たに作成されたソース接続の固有な識別子(`id`)を返します。 このIDは、後の手順でターゲット接続を作成する際に必要となります。

```json
{
    "id": "17faf955-2cf8-4b15-baf9-552cf88b1540",
    "etag": "\"2900a761-0000-0200-0000-5ed18cea0000\""
}
```

## ターゲットXDMスキーマの作成 {#target-schema}

以前の手順では、ソースデータを構造化するためにアドホックXDMスキーマを作成しました。 でソースデータを使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマも作成する必要があり [!DNL Platform]ます。 次に、このターゲットスキーマを使用して、ソースデータが含まれる [!DNL Platform] データセットを作成します。 このターゲットXDMスキーマは、XDM Individualプロファイルクラスも拡張します。

ターゲットXDMスキーマは、 [スキーマレジストリAPIに対するPOST要求を実行することで作成できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)。

If you would prefer to use the user interface in [!DNL Experience Platform], the [Schema Editor tutorial](../../../../xdm/tutorials/create-schema-ui.md) provides step-by-step instructions for performing similar actions in the Schema Editor.

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエスト例は、XDM Individualプロファイルクラスを拡張するXDMスキーマを作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "type": "object",
        "title": "Target schema for a Customer Success connector",
        "description": "Target schema for Database",
        "allOf": [
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile"
            },
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
            },
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details"
            }
        ],
        "meta:containerId": "tenant",
        "meta:resourceType": "schemas",
        "meta:xdmType": "object",
        "meta:class": "https://ns.adobe.com/xdm/context/profile"
    }'
```

**応答** 

A successful response returns details of the newly created schema including its unique identifier (`$id`). このIDは、後の手順でターゲットデータセット、マッピング、データフローを作成する際に必要となります。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/b750bd161fef405bc324d0c8809b02c494d73e60e7ae9b3e",
    "meta:altId": "_{TENANT_ID}.schemas.b750bd161fef405bc324d0c8809b02c494d73e60e7ae9b3e",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Target schema for a Customer Success connector",
    "type": "object",
    "description": "Target schema for Database",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details",
            "type": "object",
            "meta:xdmType": "object"
        }
    ],
    "refs": [
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/xdm/context/profile"
    ],
    "imsOrg": "{IMS_ORG}",
    "meta:extensible": false,
    "meta:abstract": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/profile"
    ],
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1590791550228,
        "repo:lastModifiedDate": 1590791550228,
        "xdm:createdClientId": "{CREATED_CLIENT_ID}",
        "xdm:lastModifiedClientId": "{LAST_MODIFIED_CLIENT_ID}",
        "xdm:createdUserId": "{CREATED_USER_ID}",
        "xdm:lastModifiedUserId": "{LAST_MODIFIED_USER_ID}",
        "eTag": "d730441903b95425145d9c742647ab4426d86549159182913e5f99cc904be5b1",
        "meta:globalLibVersion": "1.10.4.2"
    },
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:containerId": "tenant",
    "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## ターゲットデータセットの作成

ターゲットデータセットは、 [カタログサービスAPIに対してPOSTリクエストを実行し](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)、ペイロード内のターゲットスキーマのIDを提供することで作成できます。

**API 形式**

```http
POST catalog/dataSets
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/catalog/dataSets?requestDataSource=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Target dataset for a Customer Success connector",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/b750bd161fef405bc324d0c8809b02c494d73e60e7ae9b3e",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef.id` | ターゲット `$id` のXDMスキーマ。 |

**応答** 

A successful response returns an array containing the ID of the newly created dataset in the format `"@/datasets/{DATASET_ID}"`. データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。後の手順でターゲットデータセット接続とデータフローを作成する際に必要なターゲットデータセットIDを保存します。

```json
[
    "@/dataSets/5ed18e0f4f90b719196f44a9"
]
```

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込まれたデータが到着した宛先への接続を表します。 ターゲット接続を作成するには、データレークに関連付けられた固定接続仕様IDを指定する必要があります。 この接続仕様IDは次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

ターゲットスキーマ、ターゲットデータセット、データレークへの接続仕様IDに固有の識別子が追加されました。 これらの識別子を使用して、 [!DNL Flow Service] APIを使用してターゲット接続を作成し、受信ソースデータを含むデータセットを指定できます。

**API 形式**

```http
POST /targetConnections
```

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "baseConnectionId": "d6c3988d-14ef-4000-8398-8d14ef000021",
        "name": "Target Connection for CS",
        "description": "Target Connection for CS",
        "data": {
            "format": "parquet_xdm",
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}}/schemas/deb3e1096c35d8311b5d80868c4bd5b3cdfd4b3150e7345f",
                "version": "application/vnd.adobe.xed-full+json;version=1.0"
            }
        },
        "params": {
            "dataSetId": "5e543e8a60b15218ad44b95f"
        },
            "connectionSpec": {
            "id": "eb13cb25-47ab-407f-ba89-c0125281c563",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `data.schema.id` | ターゲット `$id` のXDMスキーマ。 |
| `params.dataSetId` | ターゲットデータセットのID。 |
| `connectionSpec.id` | Data Lakeへの固定接続仕様ID。 このIDは次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |

**応答** 

正常な応答は、新しいターゲット接続の固有な識別子(`id`)を返します。 この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "1f5af99c-f1ef-4076-9af9-9cf1ef507678",
    "etag": "\"530013e2-0000-0200-0000-5ebc4c110000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。 これは、リクエストペイロード内で定義されたデータマッピングを使用して、 [!DNL Conversion Service] APIに対するPOSTリクエストを実行することで達成されます。

**API 形式**

```http
POST /mappingSets
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "version": 0,
        "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/b750bd161fef405bc324d0c8809b02c494d73e60e7ae9b3e",
        "xdmVersion": "1.0",
        "id": null,
        "mappings": [
            {
                "destinationXdmPath": "_id",
                "sourceAttribute": "Id",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "person.name.fullName",
                "sourceAttribute": "Name",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "_repo.createDate",
                "sourceAttribute": "CreatedDate",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            }
        ]
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `xdmSchema` | ターゲット `$id` のXDMスキーマ。 |

**応答** 

正常な応答は、新たに作成されたマッピングの詳細(一意の識別子(`id`)を含む)を返します。 このIDは、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "7c3547d3cfc14f568a51c32b4c0ed739",
    "version": 0,
    "createdDate": 1590792069173,
    "modifiedDate": 1590792069173,
    "createdBy": "28AF22BA5DE6B0B40A494036@AdobeID",
    "modifiedBy": "28AF22BA5DE6B0B40A494036@AdobeID"
}
```

## データフロー仕様の取得 {#specs}

データフローは、ソースからデータを収集し、プラットフォームに取り込む役割を持ちます。 データフローを作成するには、まずFlow Service APIに対してGET要求を実行して、データフロー仕様を取得する必要があります。 データフロー仕様は、サードパーティの顧客成功システムからデータを収集する役割を持ちます。

**API 形式**

```http
GET /flowSpecs?property=name=="CRMToAEP"
```

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flowSpecs?property=name=="CRMToAEP"' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した応答は、顧客の成功システムのデータをに取り込む役割を持つデータフロー仕様の詳細を返し [!DNL Platform]ます。 このIDは、次の手順で新しいデータフローを作成する際に必要です。

```json
{
    "items": [
        {
            "id": "14518937-270c-4525-bdec-c2ba7cce3860",
            "name": "CRMToAEP",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "transformationSpecs": [
                {
                    "name": "Copy",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "properties": {
                            "deltaColumn": {
                                "type": "object",
                                "properties": {
                                    "name": {
                                        "type": "string"
                                    },
                                    "dateFormat": {
                                        "type": "string"
                                    },
                                    "timezone": {
                                        "type": "string"
                                    }
                                },
                                "required": [
                                    "name"
                                ]
                            }
                        },
                        "required": [
                            "deltaColumn"
                        ]
                    }
                },
                {
                    "name": "Mapping",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines various params required for different mapping from source to target",
                        "properties": {
                            "mappingId": {
                                "type": "string"
                            },
                            "mappingVersion": {
                                "type": "string"
                            }
                        }
                    }
                }
            ],
            "scheduleSpec": {
                "name": "PeriodicSchedule",
                "type": "Periodic",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "startTime": {
                            "description": "epoch time",
                            "type": "integer"
                        },
                        "endTime": {
                            "description": "epoch time",
                            "type": "integer"
                        },
                        "interval": {
                            "type": "integer"
                        },
                        "frequency": {
                            "type": "string",
                            "enum": [
                                "minute",
                                "hour",
                                "day",
                                "week"
                            ]
                        },
                        "backfill": {
                            "type": "boolean",
                            "default": true
                        }
                    },
                    "required": [
                        "startTime",
                        "frequency",
                        "interval"
                    ],
                    "if": {
                        "properties": {
                            "frequency": {
                                "const": "minute"
                            }
                        }
                    },
                    "then": {
                        "properties": {
                            "interval": {
                                "minimum": 15
                            }
                        }
                    },
                    "else": {
                        "properties": {
                            "interval": {
                                "minimum": 1
                            }
                        }
                    }
                }
            }
        }
    ]
}
```

## データフローの作成

データ収集に向けた最後の手順は、データフローを作成することです。 この時点で、次の必須値を準備する必要があります。

* [ソース接続ID](#source)
* [ターゲット接続ID](#target)
* [マッピング ID](#mapping)
* [データフロー仕様ID](#specs)

取り込みのスケジュールを設定するには、まず開始時間の値を秒単位のエポック時間に設定する必要があります。 次に、頻度の値を次の5つのオプションのいずれかに設定する必要があります。 `once`、、 `minute`、 `hour`、 `day`またはのいずれか `week`です。 interval値は、2つの連続したインジェスションの間の期間を指定し、1回限りのインジェストを作成する場合に、間隔を設定する必要はありません。 その他のすべての周波数の場合、間隔の値は次の値と等しいかそれ以上に設定する必要があり `15`ます。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/flows' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Creating a dataflow for a Customer Success connector",
        "description": "Creating a dataflow for a Customer Success connector",
        "flowSpec": {
            "id": "14518937-270c-4525-bdec-c2ba7cce3860",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "17faf955-2cf8-4b15-baf9-552cf88b1540"
        ],
        "targetConnectionIds": [
            "bc36ecd6-3b04-4067-b6ec-d63b04b0673d"
        ],
        "transformations": [
            {
                "name": "Copy",
                "params": {
                    "deltaColumn": {
                        "name": "updatedAt",
                        "dateFormat": "YYYY-MM-DD",
                        "timezone": "UTC"
                    }
                }
            },
            {
                "name": "Mapping",
                "params": {
                    "mappingId": "7c3547d3cfc14f568a51c32b4c0ed739",
                    "mappingVersion": "0"
                }
            }
        ],
        "scheduleParams": {
            "startTime": "1590792316",
            "frequency": "minute",
            "interval": "15",
            "backfill": "true"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `flowSpec.id` | 前の手順で取得した [フロー仕様ID](#specs) 。 |
| `sourceConnectionIds` | 前の手順で取得した [ソース接続ID](#source) 。 |
| `targetConnectionIds` | 前の手順で取得した [ターゲット接続ID](#target-connection) 。 |
| `transformations.params.mappingId` | 前の手順で取得した [マッピングID](#mapping) 。 |
| `transformations.params.deltaColum` | 新しいデータと既存のデータを区別するために指定された列。 増分データは、選択した列のタイムスタンプに基づいて取り込まれます。 でサポートされている日付形式 `deltaColumn` はで `yyyy-MM-dd HH:mm:ss`す。 |
| `transformations.params.mappingId` | データベースに関連付けられているマッピングID。 |
| `scheduleParams.startTime` | エポック時間のデータフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。 指定できる値は次のとおりです。 `once`、、 `minute`、 `hour`、 `day`またはのいずれか `week`です。 |
| `scheduleParams.interval` | この間隔は、連続する2つのフローの実行間隔を指定します。 間隔の値は、ゼロ以外の整数である必要があります。 頻度を「次の値」に設定する場合、間隔は不要 `once` です。他の頻度の値に対して、間隔は「次の値」以上に設定する必要があり `15` ます。 |

**応答** 

A successful response returns the ID `id` of the newly created dataflow.

```json
{
    "id": "e0bd8463-0913-4ca1-bd84-6309134ca1f6",
    "etag": "\"04004fe9-0000-0200-0000-5ebc4c8b0000\""
}
```

## データフローの監視

データフローを作成したら、データフローを介して取り込まれるデータを監視し、フローの実行、完了状態、エラーに関する情報を確認できます。 データフローの監視方法の詳細については、APIのデータフローの [監視に関するチュートリアルを参照してください ](../monitor.md)

## 次の手順

このチュートリアルに従って、ソースコネクタを作成し、顧客の成功システムからデータをスケジュールに基づいて収集します。 受信データは、やなどのダウンストリーム [!DNL Platform] サービスで使用でき [!DNL Real-time Customer Profile] るようになり [!DNL Data Science Workspace]ました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
* [Data Science ワークスペースの概要](../../../../data-science-workspace/home.md)

## 付録

次の節では、様々なクラウドストレージのソースコネクタと接続仕様をリストします。

### 接続の指定

| コネクタ名 | 接続仕様 |
| -------------- | --------------- |
| [!DNL Salesforce Service Cloud] | `cb66ab34-8619-49cb-96d1-39b37ede86ea` |
| [!DNL ServiceNow] | `eb13cb25-47ab-407f-ba89-c0125281c563` |