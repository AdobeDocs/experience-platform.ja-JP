---
keywords: Experience Platform;ホーム;人気のトピック;クラウドストレージデータ
solution: Experience Platform
title: Flow Service API を使用したクラウドストレージソースのデータフローの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、サードパーティのクラウドストレージからデータを取得し、ソースコネクタと API を使用して Platform に取り込む手順について説明します。
exl-id: 95373c25-24f6-4905-ae6c-5000bf493e6f
source-git-commit: fc719a4ec90c5150f129deec45da87df703ec4b5
workflow-type: tm+mt
source-wordcount: '1575'
ht-degree: 100%

---

# [!DNL Flow Service] API を使用したクラウドストレージソースのデータフローの作成

このチュートリアルでは、クラウドストレージソースからデータを取得し、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して Platform に取り込む手順について説明します。

>[!NOTE]
>
>データフローを作成するには、有効なベース接続 ID と、Platform 上の次のクラウドストレージソースのいずれかを用意しておく必要があります。<ul><li>[[!DNL Amazon S3]](../create/cloud-storage/s3.md)</li><li>[[!DNL Apache HDFS]](../create/cloud-storage/hdfs.md)</li><li>[[!DNL Azure Blob]](../create/cloud-storage/blob.md)</li><li>[[!DNL Azure Data Lake Storage Gen2]](../create/cloud-storage/adls-gen2.md)</li><li>[[!DNL Azure File Storage]](../create/cloud-storage/azure-file-storage.md)</li><li>[[!DNL FTP]](../create/cloud-storage/ftp.md)</li><li>[[!DNL Google Cloud Storage]](../create/cloud-storage/google.md)</li><li>[[!DNL Oracle Object Storage]](../create/cloud-storage/oracle-object-storage.md)</li><li>[[!DNL SFTP]](../create/cloud-storage/sftp.md)</li></ul>

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について説明します。
   - [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md)には、Schema Registry API の呼び出しを正常に実行するために知っておくべき重要な情報が含まれています。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（Accept ヘッダーと使用可能な値には特に注意を払う）が含まれます。
- [[!DNL Catalog Service]](../../../../catalog/home.md)：カタログは、 Experience Platform 内のデータの位置と系統を記録するシステムです。
- [[!DNL Batch ingestion]](../../../../ingestion/batch-ingestion/overview.md)：Batch Ingestion API を使用すると、データをバッチファイルとして Experience Platform に取り込むことができます。
- [サンドボックス](../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを個別の仮想環境に分割する仮想サンドボックスを提供し、デジタル体験アプリケーションの開発および進化を支援します。

### Platform API の使用

Platform API を正常に呼び出す方法については詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

## ソース接続の作成 {#source}

[!DNL Flow Service] API に対して POST リクエストを実行することで、ソース接続を作成することができます。ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID から構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

ファイルベースのソースには、次の列挙値を使用します。

| データ形式 | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| PARQUET | `parquet` |

すべてのテーブル形式のソースで、値を `tabular` に設定します。

- [カスタムの区切り文字付きファイルを使用したソース接続の作成](#using-custom-delimited-files)
- [圧縮ファイルを使用したソース接続の作成](#using-compressed-files)

**API 形式**

```http
POST /sourceConnections
```

### カスタムの区切り文字付きファイルを使用したソース接続の作成 {#using-custom-delimited-files}

**リクエスト**

カスタムの区切り文字が使用されている区切り文字付きのファイルは、`columnDelimiter` をプロパティとして指定することで取り込むことができます。あらゆる単一の文字の値を、列の区切り文字として使用できます。指定しない場合は、デフォルト値としてコンマ `(,)` が使用されます。

次のリクエスト例では、タブ区切りの値を使用して、区切り文字付きのファイルタイプのソース接続を作成します。

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
| `baseConnectionId` | アクセスする先のサードパーティのクラウドストレージシステムの一意の接続 ID。 |
| `data.format` | データ形式属性を定義する列挙値です。 |
| `data.columnDelimiter` | 任意の単一の文字の列の区切り文字を使用して、フラットファイルを収集できます。このプロパティは、CSV ファイルまたは TSV ファイルを取り込む場合にのみ必要です。 |
| `params.path` | アクセスするソースファイルのパス。 |
| `connectionSpec.id` | 特定のサードパーティのクラウドストレージシステムに関連付けられた接続仕様 ID。接続仕様 ID のリストについては、[付録](#appendix)を参照してください。 |

**応答**

リクエストが成功した場合は、新たに作成されたソース接続の一意の ID（`id`）が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "26b53912-1005-49f0-b539-12100559f0e2",
    "etag": "\"11004d97-0000-0200-0000-5f3c3b140000\""
}
```

### 圧縮ファイルを使用したソース接続の作成 {#using-compressed-files}

**リクエスト**

`compressionType` をプロパティとして指定することで、圧縮された JSON ファイルや区切り文字付きのファイルを取り込むこともできます。サポートされる圧縮ファイルのタイプのリストを次に示します。

- `bzip2`
- `gzip`
- `deflate`
- `zipDeflate`
- `tarGzip`
- `tar`

次のリクエスト例では、`gzip` ファイルタイプを使用して、圧縮された区切り文字付きファイルのソース接続を作成します。

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
| `data.properties.compressionType` | 取り込む圧縮ファイルのタイプを決定します。このプロパティは、圧縮された JSON ファイルまたは区切り文字付きファイルを取り込む場合にのみ必要です。 |

**応答**

リクエストが成功した場合は、新たに作成されたソース接続の一意の ID（`id`）が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "26b53912-1005-49f0-b539-12100559f0e2",
    "etag": "\"11004d97-0000-0200-0000-5f3c3b140000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[Schema Registry API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) に POST リクエストを実行することで、ターゲット XDM スキーマを作成できます。

ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../../xdm/api/schemas.md)に関するチュートリアルを参照してください。

## ターゲットデータセットの作成 {#target-dataset}

[Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) に POST リクエストを実行し、その際にペイロード内でターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込まれたデータが取り込まれる宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様 ID を提供する必要があります。この接続仕様 ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

これで、ターゲットスキーマとターゲットデータセット、およびデータレイクへの接続仕様 ID の一意の識別子が得られました。これらの識別子を使用すると、受信ソースデータを格納するデータセットを指定する [!DNL Flow Service] API を使用して、ターゲット接続を作成することができます。

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
| `data.schema.id` | ターゲット XDM スキーマの `$id`。 |
| `data.schema.version` | スキーマのバージョン番号。この値を、スキーマの最新のマイナーバージョンを返す `application/vnd.adobe.xed-full+json;version=1` に設定する必要があります。 |
| `params.dataSetId` | ターゲットデータセットの ID。 |
| `connectionSpec.id` | データレイクへの固定接続仕様 ID。この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |

**応答**

リクエストが成功した場合は、新しいターゲット接続の一意の ID（`id`）が返されます。この ID は、後の手順で必要になります。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。

マッピングセットを作成するには、ターゲット XDM スキーマ `$id` および作成するマッピングセットの詳細を入力する際に、[[!DNL Data Prep] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-prep.yaml) の `mappingSets` エンドポイントに POST リクエストを行います。

>[!TIP]
>
>クラウドストレージソースコネクタを使用する場合、JSON ファイル内の配列などの複雑なデータ型をマッピングできるようになりました。

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

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成されたマッピングの詳細が返されます。この値は、後の手順でデータフローを作成する際に必要になります。

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

データフローは、ソースからデータを収集し、それらを Platform に取り込む役割を果たします。 データフローを作成するには、まず、クラウドストレージデータの収集を実行するデータフロー仕様を取得する必要があります。

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

応答が成功すると、ソースから Platform にデータを取り込む必要があるデータフローの仕様の詳細が返されます。応答には、新しいデータフローを作成するために必要な、一意のフロー仕様 `id` が含まれます。

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

クラウドストレージデータを収集するための最後の手順は、データフローを作成することです。現時点で、次の必要な値の準備ができています。

- [ソース接続 ID](#source)
- [ターゲット接続 ID](#target)
- [マッピング ID](#mapping)
- [データフロー仕様 ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。ペイロードに前述の値を提供しながら POST リクエストを実行することで、データフローを作成することができます。

>[!NOTE]
>
>バッチ取り込みの場合、その後のデータフローでは、ソースから取り込まれるファイルが&#x200B;**最終変更日**&#x200B;のタイムスタンプに基づいて選択されます。つまり、バッチデータフローでは、新しいファイルまたは最後のデータフローの実行以降に変更されたファイルをソースから選択します。

取り込みをスケジュールするには、まず開始時刻の値をエポック時間（秒）に設定する必要があります。次に、頻度の値を次の 5 つのオプションのいずれかに設定する必要があります。`once`、`minute`、`hour`、`day` または `week`。インターバルの値には、連続した 2 回の取り込みの間の期間を指定します。1 回限りの取り込みを作成する場合は、インターバルを設定する必要はありません。その他のすべての頻度では、間隔の値を `15` 以上に設定する必要があります。

>[!IMPORTANT]
>
>[FTP コネクタ](../../../connectors/cloud-storage/ftp.md)を使用する際に、1 回限りの取り込みでデータフローをスケジュールすることを強くお勧めします。

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
                    "mappingVersion": 0
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
| `flowSpec.id` | 前の手順で取得した[フロー仕様 ID](#specs)。 |
| `sourceConnectionIds` | 前の手順で取得した[ソース接続 ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した[ターゲット接続 ID](#target-connection)。 |
| `transformations.params.mappingId` | 前の手順で取得した[マッピング ID](#mapping)。 |
| `scheduleParams.startTime` | エポック時間で表した、データフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。指定できる値は、`once`、`minute`、`hour`、`day`、`week` です。 |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。インターバルの値はゼロ以外の整数にしてください。頻度が `once` に設定されている場合、間隔は必須ではありません。また、頻度は他の頻度の値に対して、`15` よりも大きいか、等しい必要があります。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）が返されます。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## データフローの監視

データフローが作成されると、それを通して取り込まれるデータを監視し、フローの実行状況、完了状況、エラーなどの情報を確認することができます。データフローのモニタリング方法について詳しくは、[API でのデータフローの監視](../monitor.md)のチュートリアルを参照してください。

## 次の手順

このチュートリアルでは、ソースコネクタを作成し、スケジュールに従ってクラウドストレージからデータを収集しました。受信データは、[!DNL Real-time Customer Profile] および [!DNL Data Science Workspace] のようなダウンストリームの Platform サービスで使用できるようになりました。詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspace の概要](../../../../data-science-workspace/home.md)

## 付録 {#appendix}

次の節では、様々なクラウドストレージソースコネクタとその接続仕様を示します。

### 接続仕様

| コネクタ名 | 接続仕様 |
| -------------- | --------------- |
| [!DNL Amazon S3]（S3） | `ecadc60c-7455-4d87-84dc-2a0e293d997b` |
| [!DNL Amazon Kinesis]（Kinesis） | `86043421-563b-46ec-8e6c-e23184711bf6` |
| [!DNL Azure Blob]（Blob） | `4c10e202-c428-4796-9208-5f1f5732b1cf` |
| [!DNL Azure Data Lake Storage Gen2]（ADLS Gen2） | `b3ba5556-48be-44b7-8b85-ff2b69b46dc4` |
| [!DNL Azure Event Hubs]（Event Hubs） | `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |
| [!DNL Azure File Storage] | `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8` |
| [!DNL Google Cloud Storage] | `32e8f412-cdf7-464c-9885-78184cb113fd` |
| [!DNL HDFS] | `54e221aa-d342-4707-bcff-7a4bceef0001` |
| [!DNL Oracle Object Storage] | `c85f9425-fb21-426c-ad0b-405e9bd8a46c` |
| [!DNL SFTP] | `bf367b0d-3d9b-4060-b67b-0d3d9bd06094` |
