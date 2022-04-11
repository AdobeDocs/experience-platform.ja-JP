---
keywords: Experience Platform;ホーム;人気の高いトピック;マーケティング自動化システム;マーケティング自動化データの収集
solution: Experience Platform
title: Flow Service API を使用したマーケティング自動化ソースのデータフローの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、マーケティング自動化システムからデータを取得し、ソースコネクタと API を使用して Adobe Experience Platform に取り込む手順を説明します。
exl-id: f3754bd0-ed31-4bf2-8f97-975bf6a9b076
source-git-commit: 67e6de74ea8f2f4868a39ec1907ee1cac335c9f0
workflow-type: ht
source-wordcount: '1289'
ht-degree: 100%

---

# [!DNL Flow Service] API を使用したマーケティング自動化ソースのデータフロー作成

このチュートリアルでは、[[!DNL Flow Service]  API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、マーケティング自動化ソースからデータを取得し、Platform に取り込む手順について説明します。

>[!NOTE]
>
>データフローを作成するには、Platform 上で次のマーケティング自動化ソースのいずれかと有効なベース接続 ID が既にある必要があります。<ul><li>[[!DNL HubSpot]](../create/marketing-automation/hubspot.md)</li><li>[[!DNL Mailchimp Campaigns]](../create/marketing-automation/mailchimp-campaign.md)</li><li>[[!DNL Mailchimp Members]](../create/marketing-automation/mailchimp-members.md)</li><li>[[!DNL Salesforce Marketing Cloud]](../create/marketing-automation/salesforce-marketing-cloud.md)</li></ul>

## はじめに

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について説明します。
   * [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md)には、Schema Registry API の呼び出しを正常に実行するために知っておくべき重要な情報が含まれています。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（Accept ヘッダーと使用可能な値には特に注意を払う）が含まれます。
* [[!DNL Catalog Service]](../../../../catalog/home.md)：カタログは、 Experience Platform 内のデータの位置と系統を記録するシステムです。
* [[!DNL Batch ingestion]](../../../../ingestion/batch-ingestion/overview.md)：Batch Ingestion API を使用すると、データをバッチファイルとして Experience Platform に取り込むことができます。
* [サンドボックス](../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを個別の仮想環境に分割する仮想サンドボックスを提供し、デジタル体験アプリケーションの開発および進化を支援します。

### Platform API の使用

Platform API を正常に呼び出す方法については詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

##  ソース接続の作成 {#source}

[!DNL Flow Service] API に対して POST リクエストを実行することで、ソース接続を作成することができます。ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID から構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

ファイルベースのコネクタには、次の列挙値を使用します。

| データ形式 | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| PARQUET | `parquet` |

すべてのテーブルベースのコネクタで、値を`tabular`に設定します。 

**API 形式**

```https
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
        "name": "HubSpot source connection",
        "baseConnectionId": "c6d4ee17-6752-4e83-94ee-1767522e83fa",
        "description": "HubSpot source connection",
        "data": {
            "format": "tabular",
        },
        "params": {
            "path": "Hubspot.Contacts"
        },
        "connectionSpec": {
            "id": "cc6a4487-9e91-433e-a3a3-9cf6626c1806",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `baseConnectionId` | アクセスするサードパーティのマーケティング自動化システムの一意の接続 ID。 |
| `params.path` | アクセスするソースファイルのパス。 |
| `connectionSpec.id` | マーケティング自動化システムの接続仕様 ID。 |

**応答**

応答が成功すると、新しく作成されたソース接続の一意の識別子（`id`）が返されます。この値は、後の手順でターゲット接続を作成する際に必要になるので保存しておいてください。

```json
{
    "id": "f44dbef2-a4f0-4978-8dbe-f2a4f0e978cf",
    "etag": "\"5f00fba7-0000-0200-0000-5ed560520000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[Schema Registry API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) に POST リクエストを行うことで、ターゲット XDM スキーマを作成することができます。

ターゲット XDM スキーマの作成方法に関する詳細な手順については、 [API を使用したスキーマの作成](../../../../xdm/api/schemas.md)のチュートリアルを参照してください。

## ターゲットデータセットの作成 {#target-dataset}

ターゲットデータセットは、[Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) に POST リクエストを行い、ペイロードにターゲットスキーマの ID を指定することで作成することができます。

ターゲットデータセットの作成手順について詳しくは、[API を使用したデータセットの作成](../../../../catalog/api/create-dataset.md)を参照してください。

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込まれたデータが取り込まれる宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様 ID を提供する必要があります。この接続仕様 ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 

これで、ターゲットスキーマとターゲットデータセットの一意の識別子、およびデータレイクに対する接続仕様 ID が得られました。 [!DNL Flow Service] API を使用すると、受信ソースデータを格納するデータセットと共にこれらの識別子を指定することで、ターゲット接続を作成できます。

**API 形式**

```https
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
        "name": "HubSpot target connection",
        "description": "HubSpot target connection",
        "data": {
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/da411446eec78026c28d9fafd9e406e304b771d55b07b91b",
                "version": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "params": {
            "dataSetId": "5ed5639d798a22191b6987b2"
        },
        "connectionSpec": {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `data.schema.id` |  ターゲット XDM スキーマの `$id`。 |
| `data.schema.version` | スキーマのバージョン番号。この値は、スキーマの最新のマイナーバージョンを返す `application/vnd.adobe.xed-full+json;version=1` を設定する必要があります。 |
| `params.dataSetId` | ターゲットデータセットの ID。 |
| `connectionSpec.id` | データレイクへの接続に使用する接続仕様 ID。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |

```json
{
    "id": "4b3d05d8-b7aa-40de-bd05-d8b7aa80de65",
    "etag": "\"dd00a1a2-0000-0200-0000-5ed564850000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。

マッピングセットを作成するには、ターゲット XDM スキーマ `$id` と作成するマッピングセットの詳細を指定すると同時に、[[!DNL Data Prep]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-prep.yaml) の `mappingSets` エンドポイントに POST リクエストを行います。

**API 形式**

```https
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
        "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/da411446eec78026c28d9fafd9e406e304b771d55b07b91b",
        "xdmVersion": "1.0",
        "id": null,
        "mappings": [
            {
                "destinationXdmPath": "_id",
                "sourceAttribute": "Vid",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "person.name.firstName",
                "sourceAttribute": "Properties_Firstname_Value",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "_repo.createDate",
                "sourceAttribute": "Added_At",
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
| `xdmSchema` | ターゲット XDM スキーマの ID。 |

**応答**

応答が成功すると、一意の識別子（`id`）を含む、新しく作成されたマッピングの詳細が返されます。この値は、後の手順でデータフローを作成する際に必要になるので保存します。

```json
{
    "id": "500a9b747fcf4908a21917d49bd61780",
    "version": 0,
    "createdDate": 1591043336298,
    "modifiedDate": 1591043336298,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

## データフロー仕様の検索 {#specs}

データフローは、ソースからデータを収集し、それらを Platform に取り込む役割を果たします。 データフローを作成するには、まずマーケティング自動化データの収集を行うデータフロー仕様を取得する必要があります。

**API 形式**

```https
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

応答が成功すると、ソースから Platform にデータを取り込む必要があるデータフローの仕様の詳細が返されます。応答には、新しいデータフローを作成するために必要な一意のフロー仕様 `id` が含まれています。

```json
{
    "items": [
        {
            "id": "14518937-270c-4525-bdec-c2ba7cce3860",
            "name": "CRMToAEP",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "sourceConnectionSpecIds": [
                "3416976c-a9ca-4bba-901a-1f08f66978ff",
                "38ad80fe-8b06-4938-94f4-d4ee80266b07",
                "d771e9c1-4f26-40dc-8617-ce58c4b53702",
                "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
                "cc6a4487-9e91-433e-a3a3-9cf6626c1806",
                "3000eb99-cd47-43f3-827c-43caf170f015",
                "26d738e0-8963-47ea-aadf-c60de735468a",
                "74a1c565-4e59-48d7-9d67-7c03b8a13137",
                "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
                "4f63aa36-bd48-4e33-bb83-49fbcd11c708",
                "cb66ab34-8619-49cb-96d1-39b37ede86ea",
                "eb13cb25-47ab-407f-ba89-c0125281c563",
                "1f372ff9-38a4-4492-96f5-b9a4e4bd00ec",
                "37b6bf40-d318-4655-90be-5cd6f65d334b",
                "a49bcc7d-8038-43af-b1e4-5a7a089a7d79",
                "221c7626-58f6-4eec-8ee2-042b0226f03b",
                "a8b6a1a4-5735-42b4-952c-85dce0ac38b5",
                "6a8d82bc-1caf-45d1-908d-cadabc9d63a6",
                "aac9bbd4-6c01-46ce-b47e-51c6f0f6db3f",
                "8e6b41a8-d998-4545-ad7d-c6a9fff406c3",
                "ecde33f2-c56f-46cc-bdea-ad151c16cd69",
                "102706fb-a5cd-42ee-afe0-bc42f017ff43",
                "09182899-b429-40c9-a15a-bf3ddbc8ced7",
                "0479cc14-7651-4354-b233-7480606c2ac3",
                "d6b52d86-f0f8-475f-89d4-ce54c8527328",
                "a8f4d393-1a6b-43f3-931f-91a16ed857f4",
                "1fe283f6-9bec-11ea-bb37-0242ac130002"
            ],
            "targetConnectionSpecIds": [
                "c604ff05-7f1a-43c0-8e18-33bf874cb11c"
            ],
            "optionSpec": {
                "name": "OptionSpec",
                "spec": {
                    "$schema": "http://json-schema.org/draft-07/schema#",
                    "type": "object",
                    "properties": {
                        "errorDiagnosticsEnabled": {
                            "title": "Error diagnostics.",
                            "description": "Flag to enable detailed and sample error diagnostics summary.",
                            "type": "boolean",
                            "default": false
                        },
                        "partialIngestionPercent": {
                            "title": "Partial ingestion threshold.",
                            "description": "Percentage which defines the threshold of errors allowed before the run is marked as failed.",
                            "type": "number",
                            "exclusiveMinimum": 0
                        }
                    }
                }
            },
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
                        "frequency": {
                            "type": "string",
                            "enum": [
                                "once",
                                "minute",
                                "hour",
                                "day",
                                "week"
                            ]
                        },
                        "interval": {
                            "type": "integer"
                        },
                        "backfill": {
                            "type": "boolean",
                            "default": true
                        }
                    },
                    "required": [
                        "startTime",
                        "frequency"
                    ],
                    "if": {
                        "properties": {
                            "frequency": {
                                "const": "once"
                            }
                        }
                    },
                    "then": {
                        "allOf": [
                            {
                                "not": {
                                    "required": [
                                        "interval"
                                    ]
                                }
                            },
                            {
                                "not": {
                                    "required": [
                                        "backfill"
                                    ]
                                }
                            }
                        ]
                    },
                    "else": {
                        "required": [
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
            },
            "attributes": {
                "notification": {
                    "category": "sources",
                    "flowRun": {
                        "enabled": true
                    }
                }
            },
            "permissionsInfo": {
                "view": [
                    {
                        "@type": "lowLevel",
                        "name": "EnterpriseSource",
                        "permissions": [
                            "read"
                        ]
                    }
                ],
                "manage": [
                    {
                        "@type": "lowLevel",
                        "name": "EnterpriseSource",
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

## データフローの作成

マーケティング自動化データを収集するための最後の手順は、データフローを作成することです。現時点で、次の必要な値の準備ができています。

* [ソース接続 ID](#source)
* [ターゲット接続 ID](#target)
* [マッピング ID](#mapping)
* [データフロー仕様 ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。ペイロードに前述の値を提供しながら POST リクエストを実行することで、データフローを作成することができます。

取り込みをスケジュールするには、まず開始時刻の値をエポック時間（秒）に設定する必要があります。次に、頻度の値を `once`、`minute`、`hour`、`day`、`week` の 5 つのいずれかに設定する必要があります。インターバルの値には、連続した 2 回の取り込みの間の期間を指定します。1 回限りの取り込みを作成する場合は、インターバルを設定する必要はありません。その他のすべての頻度では、インターバルの値を次の値以上に設定する必要があります `15`

**API 形式**

```https
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
        "name": "HubSpot dataflow",
        "description": "collecting Hubspot.Contacts",
        "flowSpec": {
            "id": "14518937-270c-4525-bdec-c2ba7cce3860",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "f44dbef2-a4f0-4978-8dbe-f2a4f0e978cf"
        ],
        "targetConnectionIds": [
            "4b3d05d8-b7aa-40de-bd05-d8b7aa80de65"
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
                    "mappingId": "500a9b747fcf4908a21917d49bd61780",
                    "mappingVersion": "0"
                }
            }
        ],
        "scheduleParams": {
            "startTime": "1591043454",
            "frequency":"once",
            "interval":"15"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `flowSpec.id` | 前の手順で取得した[フロー仕様 ID](#specs)。 |
| `sourceConnectionIds` | 前の手順で取得した[ソース接続 ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した[ターゲット接続 ID](#target-connection)。 |
| `transformations.params.mappingId` | 前の手順で取得した[マッピング ID](#mapping)。 |
| `transformations.params.deltaColum` | 新しいデータと既存のデータを区別するために使用する、指定の列。 増分データは、選択した列のタイムスタンプに基づいて取り込まれます。 `deltaColumn` でサポートされる日付形式は `yyyy-MM-dd HH:mm:ss` です。 |
| `transformations.params.mappingId` | データベースに関連付けられたマッピング ID。 |
| `scheduleParams.startTime` | エポック時間で表した、データフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。指定できる値は `once`、`minute`、`hour`、`day`、`week` です。 |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。インターバルの値はゼロ以外の整数にしてください。インターバルは、頻度が `once` に設定されている場合は不要で、それ以外の頻度の場合は `15` 以上にする必要があります。 |

**応答**

応答に成功すると、新しく作成したデータフローの ID（`id`）が返されます。

```json
{
    "id": "e0bd8463-0913-4ca1-bd84-6309134ca1f6",
    "etag": "\"04004fe9-0000-0200-0000-5ebc4c8b0000\""
}
```

## データフローの監視

データフローが作成されると、それを通して取り込まれるデータを監視し、フローの実行状況、完了状況、エラーなどの情報を確認することができます。データフローの監視方法の詳細については、チュートリアル [API でのデータフローの監視](../monitor.md)を参照してください。

## 次の手順

このチュートリアルに従って、マーケティング自動化システムからスケジュールに従ってデータを収集するソースコネクタを作成できます。[!DNL Real-time Customer Profile] や [!DNL Data Science Workspace] など、ダウンストリームの Platform サービスで受信データを利用できるようになりました。詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
* [Data Science Workspace の概要](../../../../data-science-workspace/home.md)
