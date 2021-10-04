---
keywords: Experience Platform；ホーム；人気のあるトピック；クラウドストレージデータ；ストリーミングデータ；ストリーミング
solution: Experience Platform
title: フローサービス API を使用した生データのストリーミングデータフローの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、ストリーミングデータを取得し、ソースコネクタと API を使用して Platform に取り込む手順を説明します。
exl-id: 898df7fe-37a9-4495-ac05-30029258a6f4
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 16%

---

# [!DNL Flow Service] API を使用した生データのストリーミングデータフローの作成

このチュートリアルでは、ストリーミングソースコネクタから生データを取得し、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してExperience Platformに送信する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md):スキーマレジストリ API への呼び出しを正しく実行するために知っておく必要がある重要な情報が含まれています。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
- [[!DNL Catalog Service]](../../../../catalog/home.md):カタログは、データの場所とリネージのExperience Platformです。
- [[!DNL Streaming ingestion]](../../../../ingestion/streaming-ingestion/overview.md):Platform のストリーミング取り込みを使用すると、ユーザーはクライアントおよびサーバーサイドのデバイスから、リアルタイムでExperience Platformにデータを送信できます。
- [サンドボックス](../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../landing/api-guide.md) を参照してください。

### ソース接続の作成 {#source}

また、このチュートリアルでは、ストリーミングコネクタに有効なソース接続 ID が必要です。 この情報がない場合は、このチュートリアルを試す前に、次のストリーミングソース接続の作成に関するチュートリアルを参照してください。

- [[!DNL Amazon Kinesis]](../create/cloud-storage/kinesis.md)
- [[!DNL Azure Event Hubs]](../create/cloud-storage/eventhub.md)
- [[!DNL Google PubSub]](../create/cloud-storage/google-pubsub.md)

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマを作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。 このターゲット XDM スキーマは、XDM [!DNL Individual Profile] クラスも拡張します。

ターゲット XDM スキーマを作成するには、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の `/schemas` エンドポイントにPOSTリクエストを送信します。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエスト例は、XDM [!DNL Individual Profile] クラスを拡張する XDM スキーマを作成します。

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
        "title": "Sample schema for a streaming connector",
        "description": "Sample schema for a streaming connector",
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

正常な応答は、新しく作成されたスキーマの一意の識別子 (`$id`) を含む詳細を返します。 この ID は、後の手順で、ターゲットデータセット、マッピング、データフローを作成するために必要です。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/e45dd983026ce0daec5185cfddd48cbc0509015d880d6186",
    "meta:altId": "_{TENANT_ID}.schemas.e45dd983026ce0daec5185cfddd48cbc0509015d880d6186",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Sample schema for a streaming connector",
    "type": "object",
    "description": "Sample schema for a streaming connector",
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
        "repo:createdDate": 1604960074752,
        "repo:lastModifiedDate": 1604960074752,
        "xdm:createdClientId": "{CREATED_CLIENT_ID}",
        "xdm:lastModifiedClientId": "{MODIFIED_CLIENT_ID}",
        "xdm:createdUserId": "{CREATED_USER_ID}",
        "xdm:lastModifiedUserId": "{MODIFIED_USER_ID}",
        "eTag": "8522a151effd974429518ed90c3eaf6efc9bf6ffb6644087a85c6d4455dcd045",
        "meta:globalLibVersion": "1.16.1"
    },
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:containerId": "tenant",
    "meta:sandboxId": "{SANDBOX_ID}",
    "meta:sandboxType": "production",
    "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## ターゲットデータセットの作成

ターゲット XDM スキーマが作成され、その一意の `$id` を使用して、ソースデータを格納するターゲットデータセットを作成できます。 ターゲットデータセットを作成するには、[ カタログサービス API](https://www.adobe.io/experience-platform-apis/references/catalog/) の `dataSets` エンドポイントに対してPOSTリクエストを実行し、ペイロード内でターゲットスキーマの ID を指定します。

**API 形式**

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
        "name": "Test streaming dataset",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/e45dd983026ce0daec5185cfddd48cbc0509015d880d6186",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        },
        "tags": {
            "identity": [
            "enabled:true"
            ],
            "profile": [
            "enabled:true"
            ]
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 作成するデータセットの名前。 |
| `schemaRef.id` | データセットの基になる XDM スキーマの URI `$id`。 |
| `schemaRef.contentType` | スキーマのバージョン。 この値は `application/vnd.adobe.xed-full-notext+json;version=1` に設定する必要があります。これにより、スキーマの最新のマイナーバージョンが返されます。 詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../../../xdm/api/getting-started.md#versioning)の節を参照してください。 |

**応答**

正常な応答は、新しく作成されたデータセットの ID を `"@/datasets/{DATASET_ID}"` の形式で含む配列を返します。 データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。ターゲットデータセット ID は、後の手順で、ターゲット接続とデータフローを作成する際に必要になります。

```json
[
    "@/dataSets/5f7187bac6d00f194fb937c0"
]
```

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、Platform への宛先接続、または転送されたデータが送られる任意の場所を作成および管理します。 ターゲット接続には、データの宛先、データ形式、データフローの作成に必要なターゲット接続 ID に関する情報が含まれます。 ターゲット接続インスタンスは、テナントと IMS 組織に固有です。

ターゲット接続を作成するには、[!DNL Flow Service] API の `/targetConnections` エンドポイントにPOSTリクエストを送信します。 リクエストの一部として、データフォーマット、前の手順で取得した `dataSetId`、および [!DNL Data Lake] に関連付けられた固定接続仕様 ID を指定する必要があります。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

**API 形式**

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
        "name": "Streaming target connection",
        "description": "Streaming target connection",
        "connectionSpec": {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        },
        "data": {
            "format": "parquet_xdm",
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/e45dd983026ce0daec5185cfddd48cbc0509015d880d6186",
                "version": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "params": {
            "dataSetId": "5f7187bac6d00f194fb937c0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `connectionSpec.id` | [!DNL Data Lake] への接続に使用する接続仕様 ID。 この ID は次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |
| `data.format` | [!DNL Data Lake] に送信するデータの指定された形式。 |
| `params.dataSetId` | 前の手順で取得したターゲットデータセットの ID。 |

**応答**

正常な応答は、新しいターゲット接続の一意の識別子 (`id`) を返します。 この ID は後の手順で必要になります。

```json
{
    "id": "d9300194-6a82-4163-b001-946a821163b8",
    "etag": "\"4006d3e4-0000-0200-0000-5f7189220000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。

マッピングセットを作成するには、ターゲット XDM スキーマ `$id` と、作成するマッピングセットの詳細を指定しながら、[[!DNL Data Prep] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-prep.yaml) の `mappingSets` エンドポイントにPOSTリクエストを送信します。

**API 形式**

```http
POST /mappingSets
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/mappingSets' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "version": 0,
        "xdmSchema": "_{TENANT_ID}.schemas.e45dd983026ce0daec5185cfddd48cbc0509015d880d6186",
        "xdmVersion": "1.0",
        "mappings": [
            {
                "destinationXdmPath": "person.name.firstName",
                "sourceAttribute": "firstName",
                "identity": false,
                "version": 0
            },
            {
                "destinationXdmPath": "person.name.lastName",
                "sourceAttribute": "lastName",
                "identity": false,
                "version": 0
            }
        ]
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `xdmSchema` | ターゲット XDM スキーマの `$id`。 |

**応答**

正常な応答は、新しく作成されたマッピングの詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "380b032b445a46008e77585e046efe5e",
    "version": 0,
    "createdDate": 1604960750613,
    "modifiedDate": 1604960750613,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

## データフロー仕様のリストの取得 {#specs}

データフローは、ソースからデータを収集し、Platform に取り込みます。 GETフローを作成するには、まず [!DNL Flow Service] API に対してデータリクエストを実行して、データフロー仕様を取得する必要があります。

**API 形式**

```http
GET /flowSpecs
```

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flowSpecs' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、データフロー仕様のリストを返します。 [!DNL Amazon Kinesis]、[!DNL Azure Event Hubs] または [!DNL Google PubSub] のいずれかを使用してデータフローを作成するために取得する必要があるデータフロー仕様 ID は、`d69717ba-71b4-4313-b654-49f9cf126d7a` です。

```json
{
    "items": [
        {
            "id": "d69717ba-71b4-4313-b654-49f9cf126d7a",
            "name": "Stream data with optional transformation",
            "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
            "version": "1.0",
            "sourceConnectionSpecIds": [
                "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
                "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
                "86043421-563b-46ec-8e6c-e23184711bf6",
                "70116022-a743-464a-bbfe-e226a7f8210c"
            ],
            "targetConnectionSpecIds": [
                "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
                "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
                "db4fe783-ef79-4a12-bda9-32b2b1bc3b2c"
            ],
            "transformationSpecs": [
                {
                    "name": "Mapping",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines various params required for different mapping from source to target",
                        "properties": {
                            "mappingId": {
                                "type": "string"
                            }
                        }
                    }
                }
            ],
            "attributes": {
                "uiAttributes": {
                    "apiFeatures": {
                        "flowRunsSupported": false
                    }
                }
            },
            "permissionsInfo": {
                "view": [
                    {
                        "@type": "lowLevel",
                        "name": "StreamingSource",
                        "permissions": [
                            "read"
                        ]
                    }
                ],
                "manage": [
                    {
                        "@type": "lowLevel",
                        "name": "StreamingSource",
                        "permissions": [
                            "write"
                        ]
                    }
                ]
            }
        },
    ]
}
```

## データフローの作成

ストリーミングデータを収集する最後の手順は、データフローを作成することです。 現時点では、次の必須値が用意されています。

- [ソース接続 ID](#source)
- [ターゲット接続 ID](#target)
- [マッピング ID](#mapping)
- [データフロー仕様 ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。 データフローを作成するには、前述の値をPOST内に指定しながらペイロードリクエストを実行します。

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
        "name": "Streaming dataflow",
        "description": "Streaming dataflow",
        "flowSpec": {
            "id": "d69717ba-71b4-4313-b654-49f9cf126d7a",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "e96d6135-4b50-446e-922c-6dd66672b6b2"
        ],
        "targetConnectionIds": [
            "723222e2-6ab9-4b0b-b222-e26ab9bb0bc2"
        ],
        "transformations": [
            {
                "name": "Mapping",
                "params": {
                    "mappingId": "380b032b445a46008e77585e046efe5e",
                    "mappingVersion": 0
                }
            }
        ]
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `flowSpec.id` | 前の手順で取得した [ フロー仕様 ID](#specs)。 |
| `sourceConnectionIds` | 前の手順で取得した [ ソース接続 ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した [ ターゲット接続 ID](#target-connection)。 |
| `transformations.params.mappingId` | 前の手順で取得した [ マッピング ID](#mapping)。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローの ID(`id`) が返されます。

```json
{
    "id": "1f086c23-2ea8-4d06-886c-232ea8bd061d",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## 次の手順

このチュートリアルに従って、ストリーミングコネクタからストリーミングデータを収集するデータフローを作成しました。 受信データは、[!DNL Real-time Customer Profile] や [!DNL Data Science Workspace] など、ダウンストリームの Platform サービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspace の概要](../../../../data-science-workspace/home.md)
