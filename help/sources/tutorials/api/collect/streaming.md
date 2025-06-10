---
keywords: Experience Platform；ホーム；人気のトピック；クラウドストレージデータ；ストリーミングデータ；ストリーミング
solution: Experience Platform
title: Flow Service API を使用した、生データのストリーミングデータフローの作成
type: Tutorial
description: このチュートリアルでは、ストリーミングデータを取得し、ソースコネクタと API を使用してExperience Platformに取り込む手順について説明します。
exl-id: 898df7fe-37a9-4495-ac05-30029258a6f4
source-git-commit: bad1e0a9d86dcce68f1a591060989560435070c5
workflow-type: tm+mt
source-wordcount: '1208'
ht-degree: 35%

---

# [!DNL Flow Service] API を使用した、生データのストリーミングデータフローの作成

このチュートリアルでは、ストリーミングソースコネクタから生データを取得し、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してExperience Platformに取り込む手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について説明します。
   - [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md)には、Schema Registry API の呼び出しを正常に実行するために知っておくべき重要な情報が含まれています。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（Accept ヘッダーと使用可能な値には特に注意を払う）が含まれます。
- [[!DNL Catalog Service]](../../../../catalog/home.md)：カタログは、 Experience Platform 内のデータの位置と系統を記録するシステムです。
- [[!DNL Streaming ingestion]](../../../../ingestion/streaming-ingestion/overview.md):Experience Platformのストリーミング取得は、クライアントおよびサーバーサイドデバイスから、リアルタイムでExperience Platformにデータを送信する手段をユーザーに提供します。
- [ サンドボックス ](../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../landing/api-guide.md) を参照してください。

### ソース接続の作成 {#source}

このチュートリアルでは、ストリーミングコネクタの有効なソース接続 ID も必要です。 この情報がない場合は、このチュートリアルの内容を試す前に、ストリーミングソース接続の作成に関する次のチュートリアルを参照してください。

- [[!DNL Amazon Kinesis]](../create/cloud-storage/kinesis.md)
- [[!DNL Azure Event Hubs]](../create/cloud-storage/eventhub.md)
- [[!DNL Google PubSub]](../create/cloud-storage/google-pubsub.md)

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータをExperience Platformで使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれるExperience Platform データセットが作成されます。 このターゲット XDM スキーマは、XDM [!DNL Individual Profile] クラスも拡張します。

ターゲット XDM スキーマを作成するには、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の `/schemas` エンドポイントに POST リクエストを実行します。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエスト例では、XDM [!DNL Individual Profile] クラスを拡張する XDM スキーマを作成します。

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

リクエストが成功した場合は、一意の ID （`$id`）を含む、新しく作成されたスキーマの詳細が返されます。 この ID は、後の手順でターゲットデータセット、マッピングおよびデータフローを作成する際に必要になります。

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

ターゲット XDM スキーマを作成し、その一意のス `$id` ーマを使用して、ソースデータを含むターゲットデータセットを作成できるようになりました。 ターゲットデータセットを作成するには、[Catalog Service API](https://www.adobe.io/experience-platform-apis/references/catalog/) の `dataSets` エンドポイントに POST リクエストを実行し、その際、ペイロード内でターゲットスキーマの ID を指定します。

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
| `schemaRef.id` | データセットの基となる XDM スキーマの URI `$id`。 |
| `schemaRef.contentType` | スキーマのバージョン番号。この値は、スキーマの最新マイナーバージョンを返す `application/vnd.adobe.xed-full-notext+json;version=1` に設定する必要があります。 詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../../../xdm/api/getting-started.md#versioning)の節を参照してください。 |

**応答**

応答が成功すると、`"@/datasets/{DATASET_ID}"` 形式の新しく作成されたデータセットの ID を含む配列が返されます。 データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。 ターゲットデータセット ID は、後の手順でターゲット接続とデータフローを作成する際に必要です。

```json
[
    "@/dataSets/5f7187bac6d00f194fb937c0"
]
```

## ターゲット接続の作成 {#target-connection}

Target 接続は、Experience Platformへの宛先接続または転送されたデータが到達する場所を作成および管理します。 ターゲット接続には、データ宛先、データ形式、データフローの作成に必要なターゲット接続 ID に関する情報が含まれています。 ターゲット接続インスタンスは、テナントと組織に固有です。

ターゲット接続を作成するには、[!DNL Flow Service] API の `/targetConnections` エンドポイントに POST リクエストを実行します。 リクエストの一環として、データ形式、前の手順で取得した `dataSetId`、に関連付けられた固定接続仕様 ID を指定する必要 [!DNL Data Lake] あります。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

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
| `data.format` | データレイクに取り込むデータの、指定された形式。 |
| `params.dataSetId` | 前の手順で生成されたターゲットデータセットの ID。 **メモ**：ターゲット接続を作成する場合、有効なデータセット ID を指定する必要があります。 無効なデータセット ID は、エラーの原因となります。 |
| `connectionSpec.id` | データレイクへの接続に使用する接続仕様 ID。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |

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

マッピングセットを作成するには、[[!DNL Data Prep]  API](https://developer.adobe.com/experience-platform-apis/references/data-prep/) の `mappingSets` エンドポイントに POST リクエストを実行し、その際にターゲット XDM スキーマ `$id` および作成するマッピングセットの詳細を指定します。

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

データフローは、ソースからデータを収集し、それらをExperience Platformに取り込む役割を果たします。 データフローを作成するにはまず、[!DNL Flow Service] API に対してGET リクエストを実行し、データフローの仕様を取得する必要があります。

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

応答が成功すると、データフロー仕様のリストが返されます。 [!DNL Amazon Kinesis]、[!DNL Azure Event Hubs]、[!DNL Google PubSub] のいずれかを使用してデータフローを作成するために取得する必要があるデータフロー仕様 ID が `d69717ba-71b4-4313-b654-49f9cf126d7a` です。

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

>[!NOTE]
>
>ストリーミングデータフローを作成または更新した後、データ損失やデータ削除が発生する可能性を防ぐために、データ取り込みを 5 分間ほど一時停止する必要があります。

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

## 取り込み用にデータを投稿

取り込み用に送信できる生または XDM 準拠の JSON の例については、以下のサンプルペイロードを参照してください。

>[!NOTE]
>
>データフローの作成からストリーミングデータの取り込みまでの間には、少なくとも 5 分の遅延を追加する必要があります。 これにより、データを取り込む前に、データフローを完全に有効にできます。

次の例は、のすべてに適用されます。

- [[!DNL Amazon Kinesis]](../create/cloud-storage/kinesis.md)
- [[!DNL Azure Event Hubs]](../create/cloud-storage/eventhub.md)
- [[!DNL Google PubSub]](../create/cloud-storage/google-pubsub.md)

>[!BEGINTABS]

>[!TAB  生データ ]

```json
'{
      "name": "Johnson Smith",
      "location": {
          "city": "Seattle",
          "country": "United State of America",
          "address": "3692 Main Street"
      },
      "gender": "Male",
      "birthday": {
          "year": 1984,
          "month": 6,
          "day": 9
      }
  }'
```

>[!TAB XDM データ ]

```json
{
  "header": {
    "schemaRef": {
      "id": "https://ns.adobe.com/aepstreamingservicesint/schemas/73cae7e6db06ebca535cd973e3ece85e66253962f504e7d8",
      "contentType": "application/vnd.adobe.xed-full-notext+json; version=1.0"
    }
  },
  "body": {
    "xdmMeta": {
      "schemaRef": {
        "id": "https://ns.adobe.com/aepstreamingservicesint/schemas/73cae7e6db06ebca535cd973e3ece85e66253962f504e7d8",
        "contentType": "application/vnd.adobe.xed-full-notext+json; version=1.0"
      }
    },
    "xdmEntity": {
      "_id": "acme",
      "workEmail": {
        "address": "mike@acme.com",
        "primary": true,
        "type": "work",
        "status": "active"
      },
      "person": {
        "gender": "male",
        "name": {
          "firstName": "Mike",
          "lastName": "Wazowski"
        },
        "birthDate": "1985-01-01"
      },
      "identityMap": {
        "ecid": [
          {
            "id": "01262118050522051420082102000000000000"
          }
        ]
      }
    }
  }
}
```

>[!ENDTABS]

## 次の手順

このチュートリアルでは、ストリーミングコネクタからストリーミングデータを収集するデータフローを作成しました。 これで、[!DNL Real-Time Customer Profile] や [!DNL Data Science Workspace] などのダウンストリームのExperience Platform サービスで受信データを使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspace の概要](../../../../data-science-workspace/home.md)
