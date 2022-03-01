---
keywords: Experience Platform；ホーム；人気のトピック；クラウドストレージデータ
solution: Experience Platform
title: フローサービス API を使用したクラウドストレージソースのデータフローの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、サードパーティのクラウドストレージからデータを取得し、ソースコネクタと API を使用してそれらを Platform に取り込む手順について説明します。
exl-id: 95373c25-24f6-4905-ae6c-5000bf493e6f
source-git-commit: 67e6de74ea8f2f4868a39ec1907ee1cac335c9f0
workflow-type: tm+mt
source-wordcount: '1575'
ht-degree: 9%

---

# を使用して、クラウドストレージソースのデータフローを作成します。 [!DNL Flow Service] API

このチュートリアルでは、クラウドストレージソースからデータを取得し、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

>[!NOTE]
>
>データフローを作成するには、Platform 上の次のクラウドストレージソースのいずれかと有効なベース接続 ID が必要です。<ul><li>[[!DNL Amazon S3]](../create/cloud-storage/s3.md)</li><li>[[!DNL Apache HDFS]](../create/cloud-storage/hdfs.md)</li><li>[[!DNL Azure Blob]](../create/cloud-storage/blob.md)</li><li>[[!DNL Azure Data Lake Storage Gen2]](../create/cloud-storage/adls-gen2.md)</li><li>[[!DNL Azure File Storage]](../create/cloud-storage/azure-file-storage.md)</li><li>[[!DNL FTP]](../create/cloud-storage/ftp.md)</li><li>[[!DNL Google Cloud Storage]](../create/cloud-storage/google.md)</li><li>[[!DNL Oracle Object Storage]](../create/cloud-storage/oracle-object-storage.md)</li><li>[[!DNL SFTP]](../create/cloud-storage/sftp.md)</li></ul>

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md):Schema Registry API への呼び出しを正しく実行するために知っておく必要がある重要な情報が含まれています。 これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
- [[!DNL Catalog Service]](../../../../catalog/home.md):カタログは、データの場所とリネージのExperience Platformです。
- [[!DNL Batch ingestion]](../../../../ingestion/batch-ingestion/overview.md):バッチ取得 API を使用すると、データをバッチファイルとしてExperience Platformに取り込むことができます。
- [サンドボックス](../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../landing/api-guide.md).

## ソース接続の作成 {#source}

ソース接続を作成するには、 [!DNL Flow Service] API ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID で構成されます。

ソース接続を作成するには、 data format 属性の enum 値も定義する必要があります。

ファイルベースのソースには、次の enum 値を使用します。

| データフォーマット | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| Parquet | `parquet` |

すべてのテーブルベースのソースで、値をに設定します。 `tabular`.

- [カスタム区切りファイルを使用したソース接続の作成](#using-custom-delimited-files)
- [圧縮ファイルを使用したソース接続の作成](#using-compressed-files)

**API 形式**

```http
POST /sourceConnections
```

### カスタム区切りファイルを使用したソース接続の作成 {#using-custom-delimited-files}

**リクエスト**

区切り文字付きのファイルを取り込むには、 `columnDelimiter` プロパティとして。 任意の 1 文字の値は、列の区切り文字として使用できます。 指定しない場合は、コンマ `(,)` がデフォルト値として使用されます。

次のリクエスト例では、タブ区切り値を使用して、区切り形式のファイルタイプのソース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Cloud storage source connection for delimited files",
        "description": "Cloud storage source connector",
        "baseConnectionId": "9e2541a0-b143-4d23-a541-a0b143dd2301",
        "data": {
            "format": "delimited",
            "columnDelimiter": "\t"
        },
        "params": {
            "path": "/ingestion-demos/leads/tsv_data/*.tsv",
            "recursive": "true"
        },
        "connectionSpec": {
            "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `baseConnectionId` | アクセスするサードパーティクラウドストレージシステムの一意の接続 ID。 |
| `data.format` | データ形式属性を定義する enum 値です。 |
| `data.columnDelimiter` | 任意の 1 文字の列区切り文字を使用して、フラットファイルを収集できます。 このプロパティは、CSV または TSV ファイルを取り込む場合にのみ必要です。 |
| `params.path` | アクセスするソースファイルのパス。 |
| `connectionSpec.id` | 特定のサードパーティクラウドストレージシステムに関連付けられた接続仕様 ID。 詳しくは、 [付録](#appendix) 接続仕様 ID のリスト。 |

**応答**

正常な応答は、一意の識別子 (`id`) に含まれます。 この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "26b53912-1005-49f0-b539-12100559f0e2",
    "etag": "\"11004d97-0000-0200-0000-5f3c3b140000\""
}
```

### 圧縮ファイルを使用したソース接続の作成 {#using-compressed-files}

**リクエスト**

圧縮 JSON ファイルや区切り形式のファイルを取り込むには、 `compressionType` プロパティとして。 サポートされる圧縮ファイルの種類のリストは次のとおりです。

- `bzip2`
- `gzip`
- `deflate`
- `zipDeflate`
- `tarGzip`
- `tar`

次のリクエスト例では、 `gzip` ファイルタイプ。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Cloud storage source connection for compressed files",
        "description": "Cloud storage source connection for compressed files",
        "baseConnectionId": "9e2541a0-b143-4d23-a541-a0b143dd2301",
        "data": {
            "format": "delimited",
            "properties": {
                "compressionType": "gzip"
            }
        },
        "params": {
            "path": "/compressed/files.gzip"
        },
        "connectionSpec": {
            "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
            "version": "1.0"
        }
     }'
```

| プロパティ | 説明 |
| --- | --- |
| `data.properties.compressionType` | 取り込む圧縮ファイルの種類を決定します。 このプロパティは、圧縮 JSON または区切り文字付きファイルを取り込む場合にのみ必要です。 |

**応答**

正常な応答は、一意の識別子 (`id`) に含まれます。 この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "26b53912-1005-49f0-b539-12100559f0e2",
    "etag": "\"11004d97-0000-0200-0000-5f3c3b140000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成し、ソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

ターゲット XDM スキーマは、 [スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/).

ターゲット XDM スキーマの作成方法に関する詳細な手順については、 [API を使用したスキーマの作成](../../../../xdm/api/schemas.md).

## ターゲットデータセットの作成 {#target-dataset}

ターゲットデータセットは、 [カタログサービス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)：ペイロード内にターゲットスキーマの ID を指定します。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../../catalog/api/create-dataset.md).

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込まれたデータが格納される宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様 ID を指定する必要があります。 この接続仕様 ID は次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、ターゲットスキーマとターゲットデータセット、およびデータレイクへの接続仕様 ID の一意の識別子が得られました。 これらの識別子を使用すると、 [!DNL Flow Service] 受信ソースデータを格納するデータセットを指定する API。

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
        "name": "Target Connection for a Cloud Storage connector",
        "description": "Target Connection for a Cloud Storage connector",
        "data": {
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/995dabbea86d58e346ff91bd8aa741a9f36f29b1019138d4",
                "version": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "params": {
            "dataSetId": "5f3c3cedb2805c194ff0b69a"
        },
            "connectionSpec": {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `data.schema.id` | この `$id` ターゲット XDM スキーマの。 |
| `data.schema.version` | スキーマのバージョン。 この値を設定する必要があります `application/vnd.adobe.xed-full+json;version=1`：スキーマの最新のマイナーバージョンを返します。 |
| `params.dataSetId` | ターゲットデータセットの ID。 |
| `connectionSpec.id` | データレイクへの固定接続仕様 ID。 この ID は次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |

**応答**

正常な応答は、新しいターゲット接続の一意の識別子 (`id`) をクリックします。 この ID は、後の手順で必要になります。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。

マッピングセットを作成するには、 `mappingSets` エンドポイント [[!DNL Data Prep] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-prep.yaml) （ターゲット XDM スキーマを提供する際） `$id` 作成するマッピングセットの詳細。

>[!TIP]
>
>クラウドストレージソースコネクタを使用して、JSON ファイル内の配列などの複雑なデータ型をマッピングできます。

**API 形式**

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
        "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/995dabbea86d58e346ff91bd8aa741a9f36f29b1019138d4",
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
                "destinationXdmPath": "person.name.firstName",
                "sourceAttribute": "FirstName",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "person.name.lastName",
                "sourceAttribute": "LastName",
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

正常な応答は、新しく作成されたマッピングの詳細 ( 一意の識別子 (`id`) をクリックします。 この値は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "bf5286a9c1ad4266baca76ba3adc9366",
    "version": 0,
    "createdDate": 1597784069368,
    "modifiedDate": 1597784069368,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

## データフロー仕様の取得 {#specs}

データフローは、ソースからデータを収集し、それらを Platform に取り込む役割を果たします。 データフローを作成するには、まず、クラウドストレージデータの収集を担当するデータフロー仕様を取得する必要があります。

**API 形式**

```http
GET /flowSpecs?property=name=="CloudStorageToAEP"
```

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flowSpecs?property=name==%22CloudStorageToAEP%22' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、ソースから Platform にデータを取り込む必要があるデータフローの仕様の詳細を返します。 応答には、一意のフロー仕様が含まれます `id` 新しいデータフローを作成するために必要です。

```json
{
    "items": [
        {
            "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
            "name": "CloudStorageToAEP",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "sourceConnectionSpecIds": [
                "b3ba5556-48be-44b7-8b85-ff2b69b46dc4",
                "ecadc60c-7455-4d87-84dc-2a0e293d997b",
                "b7829c2f-2eb0-4f49-a6ee-55e33008b629",
                "4c10e202-c428-4796-9208-5f1f5732b1cf",
                "fb2e94c9-c031-467d-8103-6bd6e0a432f2",
                "32e8f412-cdf7-464c-9885-78184cb113fd",
                "b7bf2577-4520-42c9-bae9-cad01560f7bc",
                "998b8ae3-cec0-43b7-8abe-40b1eb4ee069",
                "be5ec48c-5b78-49d5-b8fa-7c89ec4569b8"
            ],
            "targetConnectionSpecIds": [
                "c604ff05-7f1a-43c0-8e18-33bf874cb11c"
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

クラウドストレージデータを収集するための最後の手順は、データフローを作成することです。 現時点では、次の必要な値が準備されています。

- [ソース接続 ID](#source)
- [ターゲット接続 ID](#target)
- [マッピング ID](#mapping)
- [データフロー仕様 ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。 データフローを作成するには、前述の値をPOST内に指定しながらペイロードリクエストを実行します。

>[!NOTE]
>
>バッチ取り込みの場合、その後のデータフローでは、それぞれの **最終変更日** タイムスタンプ。 つまり、バッチデータフローでは、新しいファイルまたは最後のデータフローの実行以降に変更されたファイルをソースから選択します。

取り込みをスケジュールするには、まず開始時刻の値をエポック時間（秒）に設定する必要があります。 次に、頻度の値を次の 5 つのオプションのいずれかに設定する必要があります。 `once`, `minute`, `hour`, `day`または `week`. 間隔値は、2 つの連続した取り込みから 1 回限りの取り込みを作成するまでの間隔を指定し、間隔を設定する必要はありません。 その他のすべての頻度では、間隔の値を次の値以上に設定する必要があります `15`.

>[!IMPORTANT]
>
>を使用する際に、1 回限りの取り込みでデータフローをスケジュールすることを強くお勧めします。 [FTP コネクタ](../../../connectors/cloud-storage/ftp.md).

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
        "name": "Cloud Storage flow to Platform",
        "description": "Cloud Storage flow to Platform",
        "flowSpec": {
            "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "26b53912-1005-49f0-b539-12100559f0e2"
        ],
        "targetConnectionIds": [
            "f7eb08fa-5f04-4e45-ab08-fa5f046e45ee"
        ],
        "transformations": [
            {
                "name": "Mapping",
                "params": {
                    "mappingId": "bf5286a9c1ad4266baca76ba3adc9366",
                    "mappingVersion": "0"
                }
            }
        ],
        "scheduleParams": {
            "startTime": "1597784298",
            "frequency":"minute",
            "interval":"30"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `flowSpec.id` | この [フロー仕様 ID](#specs) 前の手順で取得しました。 |
| `sourceConnectionIds` | この [ソース接続 ID](#source) 前の手順で取得した。 |
| `targetConnectionIds` | この [ターゲット接続 ID](#target-connection) 前の手順で取得した。 |
| `transformations.params.mappingId` | この [マッピング ID](#mapping) 前の手順で取得した。 |
| `scheduleParams.startTime` | エポックタイムでのデータフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。 指定できる値は次のとおりです。 `once`, `minute`, `hour`, `day`または `week`. |
| `scheduleParams.interval` | 間隔は、2 つの連続したフロー実行の間隔を示します。 間隔の値は、ゼロ以外の整数である必要があります。 頻度が `once` およびは次よりも大きいか等しい必要があります `15` を使用します。 |

**応答**

成功すると、ID(`id`) を含める必要があります。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## データフローの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。 データフローの監視方法の詳細については、 [API でのデータフローの監視](../monitor.md)

## 次の手順

このチュートリアルでは、ソースコネクタを作成し、スケジュールに従ってクラウドストレージからデータを収集しました。 受信データは、次のようなダウンストリームの Platform サービスで使用できるようになりました。 [!DNL Real-time Customer Profile] および [!DNL Data Science Workspace]. 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspace の概要](../../../../data-science-workspace/home.md)

## 付録 {#appendix}

次の節では、様々なクラウドストレージソースコネクタとその接続仕様を示します。

### 接続の仕様

| コネクタ名 | 接続仕様 |
| -------------- | --------------- |
| [!DNL Amazon S3] (S3) | `ecadc60c-7455-4d87-84dc-2a0e293d997b` |
| [!DNL Amazon Kinesis] (Kinesis) | `86043421-563b-46ec-8e6c-e23184711bf6` |
| [!DNL Azure Blob] (Blob) | `4c10e202-c428-4796-9208-5f1f5732b1cf` |
| [!DNL Azure Data Lake Storage Gen2] (ADLS Gen2) | `b3ba5556-48be-44b7-8b85-ff2b69b46dc4` |
| [!DNL Azure Event Hubs] （イベントハブ） | `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |
| [!DNL Azure File Storage] | `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8` |
| [!DNL Google Cloud Storage] | `32e8f412-cdf7-464c-9885-78184cb113fd` |
| [!DNL HDFS] | `54e221aa-d342-4707-bcff-7a4bceef0001` |
| [!DNL Oracle Object Storage] | `c85f9425-fb21-426c-ad0b-405e9bd8a46c` |
| [!DNL SFTP] | `bf367b0d-3d9b-4060-b67b-0d3d9bd06094` |
