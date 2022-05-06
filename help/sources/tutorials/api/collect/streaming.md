---
keywords: Experience Platform;ホーム;人気のトピック;クラウドストレージデータ；ストリーミングデータ；ストリーミング
solution: Experience Platform
title: フローサービス API を使用した生データのストリーミングデータフローの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、ストリーミングデータを取得し、ソースコネクタと API を使用して Platform に取り込む手順について説明します。
exl-id: 898df7fe-37a9-4495-ac05-30029258a6f4
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 55%

---

# を使用して、生データのストリーミングデータフローを作成します。 [!DNL Flow Service] API

このチュートリアルでは、ストリーミングソースコネクタから生データを取得し、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について説明します。
   - [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md)には、Schema Registry API の呼び出しを正常に実行するために知っておくべき重要な情報が含まれています。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（Accept ヘッダーと使用可能な値には特に注意を払う）が含まれます。
- [[!DNL Catalog Service]](../../../../catalog/home.md)：カタログは、 Experience Platform 内のデータの位置と系統を記録するシステムです。
- [[!DNL Streaming ingestion]](../../../../ingestion/streaming-ingestion/overview.md):Platform のストリーミング取り込みを使用すると、ユーザーはクライアントおよびサーバーサイドのデバイスから、リアルタイムでExperience Platformにデータを送信できます。
- [サンドボックス](../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法については詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

### ソース接続の作成 {#source}

また、このチュートリアルでは、ストリーミングコネクタに有効なソース接続 ID が必要です。 この情報がない場合は、このチュートリアルを試す前に、次のストリーミングソース接続の作成に関するチュートリアルを参照してください。

- [[!DNL Amazon Kinesis]](../create/cloud-storage/kinesis.md)
- [[!DNL Azure Event Hubs]](../create/cloud-storage/eventhub.md)
- [[!DNL Google PubSub]](../create/cloud-storage/google-pubsub.md)

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。このターゲット XDM スキーマは、XDM も拡張します [!DNL Individual Profile] クラス。

ターゲット XDM スキーマを作成するには、 `/schemas` エンドポイント [[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/).

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエスト例では、XDM を拡張する XDM スキーマを作成します [!DNL Individual Profile] クラス。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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

正常な応答は、新しく作成されたスキーマの一意の識別子 (`$id`) をクリックします。 この ID は、後の手順で、ターゲットデータセット、マッピング、データフローを作成するために必要になります。

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
    "imsOrg": "{ORG_ID}",
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

ターゲット XDM スキーマが作成され、その一意の `$id` これで、ソースデータを含むターゲットデータセットを作成できます。 ターゲットデータセットを作成するには、 `dataSets` エンドポイント [カタログサービス API](https://www.adobe.io/experience-platform-apis/references/catalog/)ペイロード内でターゲットスキーマの ID を指定する際に使用します。

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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `schemaRef.id` | URI `$id` XDM スキーマの場合、データセットの基になる。 |
| `schemaRef.contentType` | スキーマのバージョン番号。この値はに設定する必要があります。 `application/vnd.adobe.xed-full-notext+json;version=1`：スキーマの最新のマイナーバージョンを返します。 詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../../../xdm/api/getting-started.md#versioning)の節を参照してください。 |

**応答**

正常な応答は、新しく作成されたデータセットの ID をの形式で含む配列を返します `"@/datasets/{DATASET_ID}"`. データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。後の手順で、ターゲット接続とデータフローを作成するには、ターゲットデータセット ID が必要です。

```json
[
    "@/dataSets/5f7187bac6d00f194fb937c0"
]
```

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、Platform への宛先接続、または転送されたデータが送信される場所を作成および管理します。 ターゲット接続には、データ宛先、データ形式、およびデータフローの作成に必要なターゲット接続 ID に関する情報が含まれます。 Target 接続インスタンスは、テナントと IMS 組織に固有です。

ターゲット接続を作成するには、 `/targetConnections` エンドポイント [!DNL Flow Service] API リクエストの一環として、データ形式、 `dataSetId` 前の手順で取得され、 [!DNL Data Lake]. この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `connectionSpec.id` | に接続するために使用する接続仕様 ID [!DNL Data Lake]. この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |
| `data.format` | に取り込むデータの指定された形式 [!DNL Data Lake]. |
| `params.dataSetId` | 前の手順で取得したターゲットデータセットの ID。 |

**応答**

リクエストが成功した場合は、新しいターゲット接続の一意の ID（`id`）が返されます。この ID は、後の手順で必要になります。

```json
{
    "id": "d9300194-6a82-4163-b001-946a821163b8",
    "etag": "\"4006d3e4-0000-0200-0000-5f7189220000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。

マッピングセットを作成するには、[[!DNL Data Prep]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-prep.yaml) の `mappingSets` エンドポイントに POST リクエストを実行し、その際にターゲット XDM スキーマ `$id` および作成するマッピングセットの詳細を指定します。

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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答が成功すると、一意の ID（`id`）など、新しく作成されたマッピングの詳細が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

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

データフローは、ソースからデータを収集し、Platform に取り込む役割を担っています。データフローを作成するにはまず、[!DNL Flow Service] API に対して GET リクエストを実行し、データフローの仕様を取得する必要があります。

**API 形式**

```http
GET /flowSpecs
```

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flowSpecs' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、データフロー仕様のリストを返します。 任意の [!DNL Amazon Kinesis], [!DNL Azure Event Hubs]または  [!DNL Google PubSub]、 `d69717ba-71b4-4313-b654-49f9cf126d7a`.

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

ストリーミングデータを収集するための最後の手順は、データフローを作成することです。 現時点で、次の必要な値の準備ができています。

- [ソース接続 ID](#source)
- [ターゲット接続 ID](#target)
- [マッピング ID](#mapping)
- [データフロー仕様 ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。ペイロードに前述の値を提供しながら POST リクエストを実行することで、データフローを作成することができます。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/flows' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `flowSpec.id` | 前の手順で取得した[フロー仕様 ID](#specs)。 |
| `sourceConnectionIds` | 前の手順で取得した[ソース接続 ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した[ターゲット接続 ID](#target-connection)。 |
| `transformations.params.mappingId` | 前の手順で取得した[マッピング ID](#mapping)。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）が返されます。

```json
{
    "id": "1f086c23-2ea8-4d06-886c-232ea8bd061d",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## 次の手順

このチュートリアルでは、ストリーミングコネクタからストリーミングデータを収集するデータフローを作成しました。 受信データは、[!DNL Real-time Customer Profile] および [!DNL Data Science Workspace] のようなダウンストリームの Platform サービスで使用できるようになりました。詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspace の概要](../../../../data-science-workspace/home.md)
