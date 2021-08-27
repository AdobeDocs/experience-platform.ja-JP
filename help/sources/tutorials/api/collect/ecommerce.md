---
keywords: Experience Platform；ホーム；人気のあるトピック；eコマースデータの収集；eコマースデータ
solution: Experience Platform
title: ソースコネクタとAPIを使用したeコマースデータの収集
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、サードパーティのeコマースシステムからデータを取得し、ソースコネクタとAPIを使用してPlatformに取り込む手順について説明します。
exl-id: 0952f037-5e20-4d84-a2e6-2c9470f168f5
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '1523'
ht-degree: 19%

---

# ソースコネクタとAPIを使用したeコマースデータの収集

このチュートリアルでは、サードパーティの&#x200B;**[!UICONTROL eコマース]**&#x200B;システムからデータを取得し、ソースコネクタと[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して[!DNL Platform]に取り込む手順について説明します。

## はじめに

このチュートリアルでは、有効な接続を通じて&#x200B;**[!UICONTROL eコマース]**&#x200B;システムにアクセスできるほか、[!DNL Platform]に取り込むファイルに関する情報（ファイルのパスと構造を含む）を入手できる必要があります。 この情報がない場合は、このチュートリアルを試す前に、[フローサービスAPI](../explore/ecommerce.md)を使用したeコマースシステムの調査に関するチュートリアルを参照してください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   * [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマレジストリAPI](../../../../xdm/api/getting-started.md):スキーマレジストリAPIへの呼び出しを正常に実行する方法を説明します。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
* [[!DNL Catalog Service]](../../../../catalog/home.md):カタログは、内のデータの場所とリネージの記録システムで [!DNL Experience Platform]す。
* [[!DNL Batch ingestion]](../../../../ingestion/batch-ingestion/overview.md):バッチ取得APIを使用すると、データをバッチファイルと [!DNL Experience Platform] してに取り込みます。
* [[!DNL Sandboxes]](../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一のインスタンスを別々の仮想環境に分割 [!DNL Platform] し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して&#x200B;**[!UICONTROL eコマース]**&#x200B;システムに正常に接続するために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## ソース接続の作成 {#source}

[!DNL Flow Service] APIにPOSTリクエストを送信して、ソース接続を作成できます。 ソース接続は、接続ID、ソースデータファイルへのパス、接続仕様IDで構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

ファイルベースのコネクタには、次の列挙値を使用します。

| データフォーマット | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| Parquet | `parquet` |

すべてのテーブルベースのコネクタで、値を`tabular`に設定します。

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
        "name": "Shopify source connection",
        "baseConnectionId": "582f4f8d-71e9-4a5c-a164-9d2056318d6c",
        "description": "Shopify source connection",
        "data": {
            "format": "tabular"
        },
        "params": {
            "tableName": "Shopify.Orders",
            "columns": [
                {
                    "name": "Email",
                    "type": "string"
                },
                {
                    "name": "Phone",
                    "type": "string"
                },
            ]
        },
        "connectionSpec": {
            "id": "4f63aa36-bd48-4e33-bb83-49fbcd11c708",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `baseConnectionId` | **[!UICONTROL eコマース]**&#x200B;ソースの接続ID。 |
| `params.path` | ソースファイルのパス。 |
| `connectionSpec.id` | **[!UICONTROL eコマース]**&#x200B;ソースの接続仕様ID。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子(`id`)を返します。 このIDは、後の手順でターゲット接続を作成する際に必要になります。

```json
{
    "id": "c278ab14-acdf-440b-b67f-1265d15a7655",
    "etag": "\"10007c3f-0000-0200-0000-5fa9be720000\""
}
```

## ターゲットXDMスキーマの作成 {#target-schema}

[!DNL Platform]でソースデータを使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマを作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが格納される[!DNL Platform]データセットを作成します。 このターゲットXDMスキーマは、XDM [!DNL Individual Profile]クラスも拡張します。

[スキーマレジストリAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)に対してPOSTリクエストを実行すると、ターゲットXDMスキーマを作成できます。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエスト例は、XDM [!DNL Individual Profile]クラスを拡張するXDMスキーマを作成します。

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
        "title": "Shopify target XDM schema",
        "description": "Shopify target XDM schema",
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

正常な応答は、新しく作成されたスキーマの一意の識別子(`$id`)を含む詳細を返します。 このIDは、後の手順で、ターゲットデータセット、マッピング、データフローを作成する際に必要になります。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/854ddc36ad2c7bd001f66a4392575ed4004f81883328772f",
    "meta:altId": "_{TENANT_ID}.schemas.854ddc36ad2c7bd001f66a4392575ed4004f81883328772f",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Shopify target XDM schema",
    "type": "object",
    "description": "Shopify target XDM schema",
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
    "meta:sandboxId": "6b36e130-c5d7-11e9-949c-0da8d50fcac1",
    "meta:sandboxType": "production",
    "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## ターゲットデータセットの作成

[カタログサービスAPI](https://www.adobe.io/experience-platform-apis/references/catalog/)に対してPOSTリクエストを実行し、ペイロード内のターゲットスキーマのIDを指定することで、ターゲットデータセットを作成できます。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/catalog/dataSet' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Shopify target dataset",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/854ddc36ad2c7bd001f66a4392575ed4004f81883328772f",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef.id` | ターゲットXDMスキーマの`$id`。 |
| `schemaRef.contentType` | スキーマのバージョン。 この値は`application/vnd.adobe.xed-full-notext+json;version=1`に設定する必要があります。これにより、スキーマの最新のマイナーバージョンが返されます。 |

**応答**

正常な応答は、新しく作成されたデータセットのIDを`"@/datasets/{DATASET_ID}"`の形式で含む配列を返します。 データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。後の手順でターゲット接続とデータフローを作成する際に必要になるターゲットデータセットIDを保存します。

```json
[
    "@/dataSets/5fa9c083de62e418dd170b42"
]
```

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込んだデータの宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様IDを指定する必要があります。 この接続仕様IDは次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、ターゲットスキーマとターゲットデータセット、およびデータレイクへの接続仕様IDの一意の識別子が得られます。 [!DNL Flow Service] APIを使用して、これらの識別子を、受信ソースデータを格納するデータセットと共に指定することで、ターゲット接続を作成できます。

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
        "name": "Shopify target connection",
        "description": "Shopify target connection",
        "data": {
            "format": "parquet_xdm",
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/854ddc36ad2c7bd001f66a4392575ed4004f81883328772f",
                "version": "application/vnd.adobe.xed-full-notext+json; version=1"
            }
        },
        "params": {
            "dataSetId": "5fa9c083de62e418dd170b42"
        },
        "connectionSpec": {
            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `data.schema.id` | ターゲットXDMスキーマの`$id`。 |
| `data.schema.version` | スキーマのバージョン。 この値は`application/vnd.adobe.xed-full+json;version=1`に設定する必要があります。これにより、スキーマの最新のマイナーバージョンが返されます。 |
| `params.dataSetId` | ターゲットデータセットのID。 |
| `connectionSpec.id` | データレイクへの接続に使用する接続仕様ID。 このIDは次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |

**応答**

正常な応答は、新しいターゲット接続の一意の識別子(`id`)を返します。 この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "6c0ba537-a96b-4d74-8c95-450eb88baee8",
    "etag": "\"00005506-0000-0200-0000-5fa9c13c0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まずターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。 これは、リクエストペイロード内で定義されたデータマッピングを使用して、[POSTサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/mapping-service-api.yaml)に対して変換リクエストを実行することで実現されます。

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
        "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/854ddc36ad2c7bd001f66a4392575ed4004f81883328772f",
        "xdmVersion": "1.0",
        "id": null,
        "mappings": [
            {
                "destinationXdmPath": "personalEmail.address",
                "sourceAttribute": "Email",
                "identity": false,
                "version": 0
            },
            {
                "destinationXdmPath": "mobilePhone.number",
                "sourceAttribute": "Shipping_Address_Phone",
                "identity": false,
                "version": 0
            }
        ]
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `xdmSchema` | ターゲットXDMスキーマの`$id`。 |

**応答**

正常な応答は、新しく作成されたマッピングの詳細(一意の識別子(`id`)を含む)を返します。 このIDは、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "22922102bffd4369b6209c102a604062",
    "version": 0,
    "createdDate": 1604960750613,
    "modifiedDate": 1604960750613,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

## データフロー仕様の検索 {#specs}

データフローは、ソースからデータを収集し、[!DNL Platform]に取り込みます。 GETフローを作成するには、まず[!DNL Flow Service] APIに対してデータリクエストを実行して、データフロー仕様を取得する必要があります。 データフロー仕様は、**[!UICONTROL eコマース]**&#x200B;ソースからデータを収集します。

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

正常な応答は、ソースからPlatformにデータを取り込む必要があるデータフロー仕様の詳細を返します。 この応答には、新しいデータフローの作成に必要な固有のフロー仕様`id`が含まれます。

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

データを収集するための最後の手順は、データフローを作成することです。 この時点で、次の必要な値を準備しておく必要があります。

* [ソース接続ID](#source)
* [ターゲット接続ID](#target)
* [マッピング ID](#mapping)
* [データフロー仕様ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を担います。 データフローを作成するには、リクエストペイロード内に前述の値を指定してPOSTリクエストを実行します。

取り込みをスケジュールするには、まず開始時間の値を秒単位のエポック時間に設定する必要があります。 次に、頻度の値を次の5つのオプションのいずれかに設定する必要があります。`once`、`minute`、`hour`、`day`、または`week`。 interval値は、2つの連続した取り込みの間隔を指定し、1回限りの取り込みを作成する場合に、間隔を設定する必要はありません。 その他のすべての周波数の間隔の値は、`15`以上に設定する必要があります。

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
        "name": "Test Shopify dataflow",
        "description": "Shopify With mapping ingestion",
        "flowSpec": {
            "id": "14518937-270c-4525-bdec-c2ba7cce3860",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "c278ab14-acdf-440b-b67f-1265d15a7655"
        ],
        "targetConnectionIds": [
            "6c0ba537-a96b-4d74-8c95-450eb88baee8"
        ],
        "transformations": [
            {
                "name": "Mapping",
                "params": {
                    "mappingId": "22922102bffd4369b6209c102a604062",
                    "mappingVersion": "0"
                }
            }
        ],
        "scheduleParams": {
            "startTime": "1604961070",
            "frequency": "once"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `flowSpec.id` | 前の手順で取得した[フロー仕様ID](#specs)。 |
| `sourceConnectionIds` | 前の手順で取得した[ソース接続ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した[ターゲット接続ID](#target-connection)。 |
| `transformations.params.mappingId` | 前の手順で取得した[マッピングID](#mapping)。 |
| `transformations.params.mappingId` | **[!UICONTROL eコマース]**&#x200B;ソースに関連付けられたマッピングID。 |
| `scheduleParams.startTime` | エポックタイムでのデータフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する`frequency`。 指定できる値は次のとおりです。`once`、`minute`、`hour`、`day`、または`week`。 |
| `scheduleParams.interval` | この間隔は、2つの連続したフロー実行の間隔を指定します。 間隔の値はゼロ以外の整数にする必要があります。 `frequency`が`once`に設定され、他の`frequency`値の`15`以上である必要がある場合は、間隔は不要です。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローのID `id`が返されます。

```json
{
    "id": "20c115bc-46e3-40f3-bfe9-fb25abe4ba76",
    "etag": "\"030018cb-0000-0200-0000-5fa9c31a0000\""
}
```

## データフローの監視

データフローを作成したら、そのデータを通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。 データフローの監視方法について詳しくは、API ](../monitor.md)での[データフローの監視に関するチュートリアルを参照してください。

## 次の手順

このチュートリアルでは、データ&#x200B;**[!UICONTROL eコマース]**&#x200B;をスケジュールに従って収集するソースコネクタを作成しました。 受信データは、[!DNL Real-time Customer Profile]や[!DNL Data Science Workspace]など、ダウンストリームの[!DNL Platform]サービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
* [Data Science Workspace の概要](../../../../data-science-workspace/home.md)
