---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ソースコネクタとAPIを使用してCRMデータを収集する
topic: overview
translation-type: tm+mt
source-git-commit: 1fbc348f6355bbecf20616bb72193777b966b878
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 2%

---


# ソースコネクタとAPIを使用してCRMデータを収集する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、サードパーティのCRMシステムからデータを取得し、ソースコネクタとAPIを使用してプラットフォームにデータを取り込む手順を説明します。

## はじめに

このチュートリアルでは、有効な接続と、テーブルのパスや構造など、プラットフォームに組み込むテーブルに関する情報を通じて、サードパーティのCRMシステムにアクセスできる必要があります。 この情報がない場合は、このチュートリアルを試す前に、Flow Service APIを使用したCRMシステムの [詳細に関するチュートリアルを参照してください](../explore/crm.md) 。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   * [スキーマレジストリ開発ガイド](../../../../xdm/api/getting-started.md): スキーマレジストリAPIの呼び出しを正常に実行するために知っておく必要がある重要な情報が含まれます。 例えば、ユーザー `{TENANT_ID}`、「コンテナ」の概念、リクエストを行う際に必要なヘッダー（Acceptヘッダーとその可能な値に特に注意）などがあります。
* [カタログサービス](../../../../catalog/home.md): カタログは、エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。
* [バッチインジェスト](../../../../ingestion/batch-ingestion/overview.md): Batch Ingestion APIを使用すると、データをバッチファイルとしてExperience Platformに取り込むことができます。
* [サンドボックス](../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してCRMシステムに正常に接続するために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platformのすべてのリソース（フローサービスに属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## アドホックXDMクラスとスキーマの作成

ソースコネクタを介して外部データをプラットフォームに取り込むには、生のソースデータ用にアドホックXDMクラスとスキーマを作成する必要があります。

アドホッククラスとスキーマを作成するには、 [アドホックスキーマチュートリアルで概要を説明している手順に従い](../../../../xdm/tutorials/ad-hoc.md)ます。 アドホッククラスを作成する場合、ソースデータ内のすべてのフィールドをリクエスト本文内で記述する必要があります。

開発ガイドに説明されている手順に従って、アドホックスキーマを作成してから、続行します。 このチュートリアルの次の手順に進むには、アドホックスキーマの固有な識別子(`$id`)が必要です。

## ソース接続の作成 {#source}

アドホックXDMスキーマを作成した場合、Flow Service APIへのPOST要求を使用してソース接続を作成できるようになりました。 ソース接続は、接続ID、ソースデータファイル、およびソースデータを記述するスキーマへの参照で構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

フ **ァイルベースのコネクタの列挙値は、次のとおりです**。

| Data.format | 列挙値 |
| ----------- | ---------- |
| 区切りファイル | `delimited` |
| JSONファイル | `json` |
| パーケファイル | `parquet` |

すべての **テーブルベースのコネクタに** 、列挙値を使用します。 `tabular`.

**API形式**

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
        "name": "Source Connection for a CRM system",
        "baseConnectionId": "4cb0c374-d3bb-4557-b139-5712880adc55",
        "description": "Source Connection for a CRM system",
        "data": {
            "format": "tabular",
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/140c03de81b959db95879033945cfd4c",
                "version": "application/vnd.adobe.xed-full-notext+json; version=1"
            }
        },
        "params": {
            "tableName": "{TABLE_PATH}",
            "columns": [
                {
                    "name": "first_name",
                    "type": "string",
                    "meta": {
                        "originalType": "String"
                    }
                },
                {
                    "name": "last_name",
                    "type": "string",
                    "meta": {
                        "originalType": "String"
                    }
                },
                {
                    "name": "email",
                    "type": "string",
                    "meta": {
                        "originalType": "String"
                    }
                }
            ]
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `baseConnectionId` | アクセスするサードパーティCRMシステムの一意の接続ID。 |
| `data.schema.id` | アドホックXDMスキーマのID。 |
| `params.path` | ソースファイルのパス。 |
| `connectionSpec.id` | 特定のサードパーティCRMシステムに関連付けられている接続仕様ID。 接続仕様IDのリストについては、 [付録](#appendix) を参照してください。 |

**応答**

正常な応答は、新たに作成されたソース接続の固有な識別子(`id`)を返します。 このIDは、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "9a603322-19d2-4de9-89c6-c98bd54eb184"
    "etag": "\"4a00038b-0000-0200-0000-5ebc47fd0000\""
}
```

## ターゲットXDMスキーマの作成 {#target}

以前の手順では、ソースデータを構造化するためにアドホックXDMスキーマを作成しました。 Platformでソースデータを使用するには、必要に応じてソースデータを構造化するためのターゲットスキーマも作成する必要があります。 次に、このターゲットスキーマを使用して、ソースデータが含まれるプラットフォームデータセットを作成します。

ターゲットXDMスキーマは、 [スキーマレジストリAPIに対してPOST要求を実行することで作成できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)。 Experience Platformでユーザーインターフェイスを使用したい場合は、 [スキーマエディターのチュートリアル](../../../../xdm/tutorials/create-schema-ui.md) （英語のみ）に、スキーマエディターで同様の操作を実行するための手順を順を追って説明しています。

**API形式**

```http
POST /schemaregistry/tenant/schemas
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
        "title": "Target schema for CRM source connector",
        "description": "",
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

成功した応答は、新たに作成されたスキーマの詳細(一意の識別子(`$id`)を含む)を返します。 このIDは、後の手順でターゲットデータセット、マッピング、データフローを作成する際に必要となります。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/417a33eg81a221bd10495920574gfa2d",
    "meta:altId": "{TENANT_ID}.schemas.417a33eg81a221bd10495920574gfa2d",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Target schema for CRM source connector",
    "description": "",
    "type": "object",
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
    "meta:xdmType": "object",
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details"
    ],
    "meta:containerId": "tenant",
    "meta:registryMetadata": {
        "eTag": "6m/FrIlXYU2+yH6idbcmQhKSlMo="
    }
}
```

## ターゲットデータセットの作成

ターゲットデータセットは、 [カタログサービスAPIに対してPOSTリクエストを実行し](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)、ペイロード内のターゲットスキーマのIDを提供することで作成できます。

**API形式**

```http
POST /catalog/dataSets
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
        "name": "Target Dataset for CRM data",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/417a33eg81a221bd10495920574gfa2d",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `schemaRef.id` | ターゲットXDMスキーマのID。 |

**応答**

正常に完了すると、新しく作成されたデータセットのIDを含む配列が形式で返され `"@/datasets/{DATASET_ID}"`ます。 データセットIDは、API呼び出しでデータセットを参照するために使用される、読み取り専用の、システム生成の文字列です。 ターゲットデータセットIDは、後の手順でターゲット接続とデータフローを作成する際に必要となります。

```json
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```

## ターゲット接続の作成

ターゲット接続は、取り込まれたデータが到着した宛先への接続を表します。 ターゲット接続を作成するには、データレークに関連付けられた固定接続仕様IDを指定する必要があります。 この接続仕様IDは次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、データセットベースの接続、ターゲットスキーマ、ターゲットデータセットに対して一意のIDを割り当てることができます。 これらの識別子を使用して、Flow Service APIを使用してターゲット接続を作成し、受信ソースデータを含むデータセットを指定できます。

**API形式**

```http
POST /targetConnections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Target Connection for a CRM connector",
        "description": "Target Connection for CRM data",
        "data": {
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/417a33eg81a221bd10495920574gfa2d",
                "version": "application/vnd.adobe.xed-full+json;version=1.0"
            }
        },
        "params": {
            "dataSetId": "5c8c3c555033b814b69f947f"
        },
        "connectionSpec": {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `data.schema.id` | ターゲット `$id` のXDMスキーマ。 |
| `params.dataSetId` | ターゲットデータセットのID。 |
| `connectionSpec.id` | Data Lakeへの固定接続仕様ID。 このIDは次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |

```json
{
    "id": "4ee890c7-519c-4291-bd20-d64186b62da8",
    "etag": "\"2a007aa8-0000-0200-0000-5e597aaf0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。 これは、要求ペイロード内で定義されたデータマッピングを使用して、Conversion Service APIに対するPOST要求を実行することで達成されます。

**API形式**

```http
POST /conversion/mappingSets
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
        "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/417a33eg81a221bd10495920574gfa2d",
        "xdmVersion": "1.0",
        "id": null,
        "mappings": [
            {
                "destinationXdmPath": "person.name.firstName",
                "sourceAttribute": "first_name",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "person.name.lastName",
                "sourceAttribute": "last_name",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "personalEmail.address",
                "sourceAttribute": "email",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            }
        ]
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `xdmSchema` | ターゲットXDMスキーマのID。 |

**応答**

正常な応答は、新たに作成されたマッピングの詳細(一意の識別子(`id`)を含む)を返します。 この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "ab91c736-1f3d-4b09-8424-311d3d3e3cea",
    "version": 1,
    "createdDate": 1568047685000,
    "modifiedDate": 1568047703000,
    "inputSchemaRef": {
        "id": null,
        "contentType": null
    },
    "outputSchemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/417a33eg81a221bd10495920574gfa2d",
        "contentType": "1.0"
    },
    "mappings": [
        {
            "id": "7bbea5c0f0ef498aa20aa2e2e5c22290",
            "version": 0,
            "createdDate": 1568047685000,
            "modifiedDate": 1568047685000,
            "sourceType": "text/x.schema-path",
            "source": "Id",
            "destination": "_id",
            "identity": false,
            "primaryIdentity": false,
            "matchScore": 0.0,
            "sourceAttribute": "Id",
            "destinationXdmPath": "_id"
        },
        {
            "id": "def7fd7db2244f618d072e8315f59c05",
            "version": 0,
            "createdDate": 1568047685000,
            "modifiedDate": 1568047685000,
            "sourceType": "text/x.schema-path",
            "source": "FirstName",
            "destination": "person.name.firstName",
            "identity": false,
            "primaryIdentity": false,
            "matchScore": 0.0,
            "sourceAttribute": "FirstName",
            "destinationXdmPath": "person.name.firstName"
        },
        {
            "id": "e974986b28c74ed8837570f421d0b2f4",
            "version": 0,
            "createdDate": 1568047685000,
            "modifiedDate": 1568047685000,
            "sourceType": "text/x.schema-path",
            "source": "LastName",
            "destination": "person.name.lastName",
            "identity": false,
            "primaryIdentity": false,
            "matchScore": 0.0,
            "sourceAttribute": "LastName",
            "destinationXdmPath": "person.name.lastName"
        }
    ],
    "status": "PUBLISHED",
    "xdmVersion": "1.0",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/2574494fdb01fa14c25b52d717ccb828",
        "contentType": "1.0"
    },
    "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/2574494fdb01fa14c25b52d717ccb828"
}
```

## データフロー仕様の取得 {#specs}

データフローは、ソースからデータを収集し、プラットフォームに取り込む役割を持ちます。 データフローを作成するには、まずCRMデータの収集を担当するデータフロー仕様を取得する必要があります。

**API形式**

```http
GET /flowSpecs?property=name=="CRMToAEP"
```

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flowSpecs?property=name==%22CRMToAEP%22' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答を得ると、CRMシステムからプラットフォームにデータを取り込む役割を持つデータフロー仕様の詳細が返されます。 このIDは、次の手順で新しいデータフローを作成する際に必要です。

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

CRMデータを収集する最後の手順は、データフローを作成することです。 現時点では、次の必須の値を用意しておきます。

* [ソース接続ID](#source)
* [ターゲット接続ID](#target)
* [マッピング ID](#mapping)
* [データフロー仕様ID](#specs)

データフローは、ソースからのデータのスケジュールおよび収集を担当します。 データフローを作成するには、ペイロード内で前述の値を指定しながらPOSTリクエストを実行します。

取り込みのスケジュールを設定するには、まず開始時間の値を秒単位のエポック時間に設定する必要があります。 次に、頻度の値を次の5つのオプションのいずれかに設定する必要があります。 `once`、、 `minute`、 `hour`、 `day`またはのいずれか `week`です。 interval値は、2つの連続したインジェスションの間の期間を指定し、1回限りのインジェストを作成する場合に、間隔を設定する必要はありません。 その他のすべての周波数の場合、間隔の値は次の値と等しいかそれ以上に設定する必要があり `15`ます。

**API形式**

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
        "name": "Dataflow between a CRM system and Platform",
        "description": "Inbound data to Platform",
        "flowSpec": {
            "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "9a603322-19d2-4de9-89c6-c98bd54eb184"
        ],
        "targetConnectionIds": [
            "4ee890c7-519c-4291-bd20-d64186b62da8"
        ],
        "transformations": [
            {
                "name": "Mapping",
                "params": {
                    "mappingId": "ab91c736-1f3d-4b09-8424-311d3d3e3cea"
                }
            }
        ],
        "scheduleParams": {
            "startTime": "1567411548",
            "frequency":"minute",
            "interval":"30"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `flowSpec.id` | 前の手順で取得したフロー仕様ID。 |
| `sourceConnectionIds` | 前の手順で取得したソース接続ID。 |
| `targetConnectionIds` | 前の手順で取得したターゲット接続ID。 |
| `transformations.params.mappingId` | 前の手順で取得したマッピングID。 |
| `scheduleParams.startTime` | データフローの開始時間（秒単位）。 |
| `scheduleParams.frequency` | 選択可能な頻度の値は次のとおりです。 `once`、、 `minute`、 `hour`、 `day`またはのいずれか `week`です。 |
| `scheduleParams.interval` | この間隔は、連続する2つのフローの実行間隔を指定します。 間隔の値は、ゼロ以外の整数である必要があります。 頻度を「次の値」に設定する場合、間隔は不要 `once` です。他の頻度の値に対して、間隔は「次の値」以上に設定する必要があ `15` ります。 |

**応答**

正常な応答は、新しく作成されたデータフローのID(`id`)を返します。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
    "etag": "\"04004fe9-0000-0200-0000-5ebc4c8b0000\""

}
```

## 次の手順

このチュートリアルに従って、CRMシステムからデータを収集するソースコネクタをスケジュールに基づいて作成しました。 受信データは、リアルタイム顧客プロファイルやデータサイエンスワークスペースなどのダウンストリームプラットフォームサービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
* [Data Science Workspaceの概要](../../../../data-science-workspace/home.md)

## 付録

次の節では、様々なCRMソースコネクタとその接続仕様をリストします。

### 接続の指定

| コネクタ名 | 接続仕様 |
| -------------- | --------------- |
| Microsoft Dynamics | `38ad80fe-8b06-4938-94f4-d4ee80266b07` |
| Salesforce | `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` |
