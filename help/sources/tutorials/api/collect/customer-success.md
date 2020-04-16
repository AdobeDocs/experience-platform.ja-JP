---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ソースコネクタとAPIを使用して顧客成功システムからデータを収集する
topic: overview
translation-type: tm+mt
source-git-commit: c216e321fd913773578107df027d953f10d99a36

---


# ソースコネクタとAPIを使用して顧客成功システムからデータを収集する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、顧客の成功(「CS」)システムからデータを取得し、ソースコネクタとAPIを通じてプラットフォームに取り込む手順を説明します。

## はじめに

このチュートリアルでは、有効なベース接続と、ファイルのパスや構造など、プラットフォームに取り込むファイルに関する情報を通じて、サードパーティのCSシステムにアクセスできる必要があります。 この情報がない場合は、このチュートリアルを試す前に、Flow Service APIを使用し [たデータベースまたはNoSQLシステムの調査に関するチュートリアルを参照](../explore/customer-success.md) してください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md):スキーマレジストリAPIの呼び出しを正常に実行するために知っておく必要がある重要な情報が含まれます。 これには、ユーザ `{TENANT_ID}`ー、「コンテナ」の概念、リクエストを行うために必要なヘッダー（Acceptヘッダーとその可能な値に特に注意）が含まれます。
* [Catalog Service](../../../../catalog/home.md):カタログは、エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。
* [バッチインジェスト](../../../../ingestion/batch-ingestion/overview.md):Batch Ingestion APIを使用すると、データをバッチファイルとしてExperience Platformに取り込むことができます。
* [サンドボックス](../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してCSシステムに正常に接続するために知っておく必要のある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## アドホックXDMクラスの作成とスキーマ

ソースコネクタを介して外部データをプラットフォームに取り込むには、生のソースデータ用のアドホックXDMクラスとスキーマを作成する必要があります。

アドホッククラスとスキーマを作成するには、アドホックスキーマのチュートリアルで概要を説 [明する手順に従います](../../../../xdm/tutorials/ad-hoc.md)。 アドホッククラスを作成する場合、ソースデータ内のすべてのフィールドをリクエスト本文内で記述する必要があります。

アドホックスキーマを作成するまで、開発ガイドで説明されている手順に従います。 アドホックスキーマの固有な識別子(`$id`)を取得して保存し、このチュートリアルの次の手順に進みます。

## ソース接続の作成 {#source}

アドホックXDMスキーマを作成すると、Flow Service APIへのPOST要求を使用してソース接続を作成できるようになります。 ソース接続は、ベース接続、ソースデータファイル、およびソースデータを記述するスキーマへの参照で構成されます。

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
        "name": "Source connection for CS",
        "baseConnectionId": "60a5c8b9-3c30-43ba-a5c8-b93c3093ba66",
        "description": "Source Connection for CS to ingest Account",
        "data": {
            "format": "parquet_xdm",
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/classes/51dd6dce662f8aebe4353c74bbb49c77cb3cbcd6c6b29021",
                "version": "application/vnd.adobe.xed-full-notext+json; version=1"
            }
        },
        "params": {
            "path": "Account"
        },
        "connectionSpec": {
            "id": "eb13cb25-47ab-407f-ba89-c0125281c563",
            "version": "1.0"
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `baseConnectionId` | CSシステムのベース接続のID |
| `data.schema.id` | アドホ `$id` ックXDMスキーマの |
| `params.path` | ソースファイルのパス。 |
| `connectionSpec.id` | CSシステムの接続仕様ID。 |

**応答**

成功した応答は、新しく作成されたソース接続の`id`固有の識別子()を返します。 このIDは、後の手順で接続を作成する際に必要なターゲットです。

```json
{
    "id": "01b7cbea-cf18-4552-b7cb-eacf18055294",
    "etag": "\"2103ac94-0000-0200-0000-5e543ad70000\""
}
```

## XDMターゲットの作成スキーマ {#target}

以前の手順では、ソースデータを構造化するためのアドホックXDMスキーマを作成していました。 ソースデータをPlatformで使用するには、必要に応じてソースデータを構成するターゲットスキーマも作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれるプラットフォームデータセットを作成します。 このターゲットXDMスキーマは、XDM Individualプロファイルクラスも拡張します。

ターゲットXDMスキーマは、 [スキーマレジストリAPIに対してPOST要求を実行することで作成できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)。

Experience Platformでスキーマインターフェイスを使用したい場合は、スキーマエディターのチュートリアルで [](../../../../xdm/tutorials/create-schema-ui.md) 、同様の操作を実行するための手順を順を追って説明します。

**API形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエスト例は、XDM Individualスキーマクラスを拡張するXDMプロファイルを作成します。

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
        "title": "Target schema for CS",
        "description": "Target schema for CS",
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
    }'
```

**応答**

成功した応答は、新たに作成されたスキーマの詳細(一意の識別子(`$id`))を返します。 このIDは、後の手順で、ターゲットデータセット、マッピング、データフローを作成する際に必要です。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/deb3e1096c35d8311b5d80868c4bd5b3cdfd4b3150e7345f",
    "meta:altId": "_{TENANT_ID}.schemas.deb3e1096c35d8311b5d80868c4bd5b3cdfd4b3150e7345f",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Target schema for CS",
    "type": "object",
    "description": "Target schema for CS",
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
        "repo:createdDate": 1582578764554,
        "repo:lastModifiedDate": 1582578764554,
        "xdm:createdClientId": "{CREATED_CLIENT_ID}",
        "xdm:lastModifiedClientId": "{LAST_MODIFIED_CLIENT_ID}",
        "xdm:createdUserId": "{CREATED_USER_ID}",
        "xdm:lastModifiedUserId": "{LAST_MODIFIED_USER_ID}",
        "eTag": "213e4bf7cbac74e3a9e6f988da321e2f7353acacd9ea651a5652bd49b28b8d2a"
    },
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:containerId": "tenant",
    "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## ターゲットデータセットの作成

ターゲットデータセットは、 [Catalog Service APIに対してPOSTリクエストを実行し、ペイロード内のターゲットスキーマのIDを提供することで](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)、作成できます。

**API形式**

```http
POST /dataSets
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
        "name": "Target Dataset for CS",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/deb3e1096c35d8311b5d80868c4bd5b3cdfd4b3150e7345f",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef.id` | XDM `$id` ターゲットのスキーマ。 |

**応答**

正常に完了すると、新しく作成されたデータセットのIDを形式で含む配列が返されま `"@/datasets/{DATASET_ID}"`す。 データセットIDは、API呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。 後の手順で必要なターゲットデータセットIDを保存し、ターゲット接続とデータフローを作成します。

```json
[
    "@/dataSets/5e543e8a60b15218ad44b95f"
]
```

## データセットベース接続の作成

外部データをプラットフォームに取り込むには、まず、エクスペリエンスプラットフォームのデータセットベース接続を取得する必要があります。

データセットベースの接続を作成するには、「データセットベースの接続」のチュートリ [アルで概要を説明する手順に従いま](../create-dataset-base-connection.md)す。

データセットベースの接続を作成するまで、開発者ガイドで説明されている手順に従います。 一意の識別子(`$id`)を取得して保存し、次の手順でそれをベース接続IDとして使用して、ターゲット接続を作成します。

## ターゲット接続

これで、データセットベース接続の固有の識別子、ターゲットスキーマ、ターゲットデータセットが得られます。 Flow Service APIを使用してターゲット接続を作成し、受信ソースデータを含むデータセットを指定できるようになりました。

**API形式**

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
| `baseConnectionId` | データセットベース接続のID。 |
| `data.schema.id` | XDM `$id` ターゲットのスキーマ。 |
| `params.dataSetId` | ターゲットデータセットのID。 |
| `connectionSpec.id` | 顧客成功システムの接続指定ID。 |

**応答**

正常な応答は、新しいターゲット接続の固有な識別子(`id`)を返します。 この値は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "5ebaf0b3-66dc-46bf-baf0-b366dc76bfd5",
    "etag": "\"5d02211d-0000-0200-0000-5e543f0f0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが対応するターゲットスキーマにマップする必要があります。 これは、リクエストペイロード内で定義されたデータマッピングを使用して、コンバージョンサービスAPIに対するPOSTリクエストを実行することで達成されます。

**API形式**

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
        "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/deb3e1096c35d8311b5d80868c4bd5b3cdfd4b3150e7345f",
        "xdmVersion": "1.0",
        "id": null,
        "mappings": [
            {
                "destinationXdmPath": "person.name",
                "sourceAttribute": "Name",
                "identity": false,
                "identityGroup": null,
                "namespaceCode": null,
                "version": 0
            },
            {
                "destinationXdmPath": "mobilePhone.number",
                "sourceAttribute": "Phone",
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
| -------- | ----------- |
| `xdmSchema` | XDM `$id` ターゲットのスキーマ。 |

**応答**

成功した応答は、新しく作成されたマッピングの詳細(一意の識別子(`id`)を含む)を返します。 このIDは、後の手順でデータフローを作成する必要があります。

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
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/efea012ad5deefcdf51afd23ceb3583f",
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

## データフロー仕様の検索 {#specs}

データフローは、ソースからデータを収集し、プラットフォームに取り込む役割を持ちます。 データフローを作成するには、まずフローサービスAPIに対してGET要求を実行して、データフロー仕様を取得する必要があります。 データフロー仕様は、外部データベースまたはNoSQLシステムからデータを収集します。

**API形式**

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

正常に応答すると、CSシステムのデータをプラットフォームに取り込む処理を行うデータフロー仕様の詳細が返されます。 このIDは、新しいデータフローを作成する次の手順で必要です。

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

データを収集する最後の手順は、データフローを作成することです。 この時点で、次の必須の値を準備する必要があります。

* [ソース接続ID](#source)
* [ターゲット接続ID](#target)
* [マッピング ID](#mapping)
* [データフロー仕様ID](#specs)

データフローは、ソースからのデータのスケジュールと収集を行います。 データフローを作成するには、ペイロード内で前述の値を指定しながらPOSTリクエストを実行します。

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
        "name": "Dataflow between database or NoSQL Platform",
        "description": "Inbound data to Platform",
        "flowSpec": {
            "id": "14518937-270c-4525-bdec-c2ba7cce3860",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "01b7cbea-cf18-4552-b7cb-eacf18055294"
        ],
        "targetConnectionIds": [
            "5ebaf0b3-66dc-46bf-baf0-b366dc76bfd5"
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
                    "mappingId": "ab91c736-1f3d-4b09-8424-311d3d3e3cea",
                    "mappingVersion": "0"
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

**応答**

成功した応答は、新しく作成されたデ `id` ータフローのIDを返します。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```

## 次の手順

このチュートリアルに従って、CSシステムからデータをスケジュールに基づいて収集するソースコネクタを作成しました。 受信データは、リアルタイム顧客情報やData Science Workspaceなどのダウンストリームプラットフォームサービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
* [Data Science Workspaceの概要](../../../../data-science-workspace/home.md)