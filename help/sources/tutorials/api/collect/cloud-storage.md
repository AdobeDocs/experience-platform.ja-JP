---
keywords: Experience Platform；ホーム；人気のあるトピック；クラウドストレージデータ
solution: Experience Platform
title: ソースコネクタと API を使用したクラウドストレージデータの収集
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、サードパーティのクラウドストレージからデータを取得し、ソースコネクタと API を使用してそれらを Platform に取り込む手順について説明します。
exl-id: 95373c25-24f6-4905-ae6c-5000bf493e6f
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '1792'
ht-degree: 17%

---

# ソースコネクタと API を使用したクラウドストレージデータの収集

このチュートリアルでは、サードパーティのクラウドストレージからデータを取得し、ソースコネクタと [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を通じてそれらを Platform に取り込む手順を説明します。

## はじめに

このチュートリアルでは、有効な接続を通じてサードパーティのクラウドストレージにアクセスし、Platform に取り込むファイルに関する情報（ファイルのパスや構造など）を入手する必要があります。 この情報がない場合は、このチュートリアルを試す前に、 [!DNL Flow Service] API](../explore/cloud-storage.md) を使用したサードパーティのクラウドストレージの調査に関する [ のチュートリアルを参照してください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM) System]](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマレジストリ開発者ガイド](../../../../xdm/api/getting-started.md):スキーマレジストリ API への呼び出しを正しく実行するために知っておく必要がある重要な情報が含まれています。これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。
- [[!DNL Catalog Service]](../../../../catalog/home.md):カタログは、データの場所とリネージのExperience Platformです。
- [[!DNL Batch ingestion]](../../../../ingestion/batch-ingestion/overview.md):バッチ取得 API を使用すると、データをバッチファイルとしてExperience Platformに取り込むことができます。
- [サンドボックス](../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。以下の節では、[!DNL Flow Service] API を使用してクラウドストレージに正常に接続するために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service] に属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

- `Content-Type: application/json`

## ソース接続の作成 {#source}

[!DNL Flow Service] API にPOSTリクエストを実行して、ソース接続を作成できます。 ソース接続は、接続 ID、ソースデータファイルのパス、接続仕様 ID で構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

ファイルベースのソースには、次の列挙値を使用します。

| データフォーマット | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| Parquet | `parquet` |

すべてのテーブルベースのソースで、値を `tabular` に設定します。

- [カスタム区切りファイルを使用したソース接続の作成](#using-custom-delimited-files)
- [圧縮ファイルを使用したソース接続の作成](#using-compressed-files)

**API 形式**

```http
POST /sourceConnections
```

### カスタム区切りファイルを使用したソース接続の作成 {#using-custom-delimited-files}

**リクエスト**

区切り文字付きファイルをカスタム区切り文字で取り込むには、プロパティとして `columnDelimiter` を指定します。 任意の 1 文字の値は、列の区切り文字として許容されます。 指定しない場合、コンマ `(,)` がデフォルト値として使用されます。

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
| `data.format` | データ形式属性を定義する enum 値。 |
| `data.columnDelimiter` | 任意の 1 文字の列区切り文字を使用して、フラットファイルを収集できます。 このプロパティは、CSV または TSV ファイルを取り込む場合にのみ必要です。 |
| `params.path` | アクセスするソースファイルのパス。 |
| `connectionSpec.id` | 特定のサードパーティクラウドストレージシステムに関連付けられている接続仕様 ID。 接続仕様 ID のリストについては、[ 付録 ](#appendix) を参照してください。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子 (`id`) を返します。 この ID は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "26b53912-1005-49f0-b539-12100559f0e2",
    "etag": "\"11004d97-0000-0200-0000-5f3c3b140000\""
}
```

### 圧縮ファイルを使用したソース接続の作成 {#using-compressed-files}

**リクエスト**

また、プロパティとして `compressionType` を指定することで、圧縮された JSON ファイルや区切られたファイルを取り込むこともできます。 サポートされる圧縮ファイルの種類は次のとおりです。

- `bzip2`
- `gzip`
- `deflate`
- `zipDeflate`
- `tarGzip`
- `tar`

次のリクエスト例では、`gzip` ファイルタイプを使用して圧縮された区切りファイルのソース接続を作成します。

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
                "compressionType" : "gzip"
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
| `data.properties.compressionType` | 取り込む圧縮ファイルの種類を指定します。 このプロパティは、圧縮された JSON または区切り形式のファイルを取り込む場合にのみ必要です。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子 (`id`) を返します。 この ID は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "26b53912-1005-49f0-b539-12100559f0e2",
    "etag": "\"11004d97-0000-0200-0000-5f3c3b140000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマを作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[ スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) に対してPOSTリクエストを実行すると、ターゲット XDM スキーマを作成できます。

**API 形式**

```http
POST /schemaregistry/tenant/schemas
```

**リクエスト**

次のリクエスト例は、XDM Individual Profile クラスを拡張する XDM スキーマを作成します。

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
        "title": "Target schema for a Cloud Storage connector",
        "description": "Target schema for a Cloud Storage connector",
        "allOf": [
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile"
            },
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
            },
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details"
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
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/995dabbea86d58e346ff91bd8aa741a9f36f29b1019138d4",
    "meta:altId": "_{TENANT_ID}.schemas.995dabbea86d58e346ff91bd8aa741a9f36f29b1019138d4",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Target schema cloud storage",
    "type": "object",
    "description": "Target schema for cloud storage",
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
        "repo:createdDate": 1597783248870,
        "repo:lastModifiedDate": 1597783248870,
        "xdm:createdClientId": "{CREATED_CLIENT_ID}",
        "xdm:lastModifiedClientId": "{LAST_MODIFIED_CLIENT_ID}",
        "xdm:createdUserId": "{CREATED_USER_ID}",
        "xdm:lastModifiedUserId": "{LAST_MODIFIED_USER_ID}",
        "eTag": "596661ec6c7a9c6ae530676e98290a4a58ca29540ed92489cf4478b2bf013a65",
        "meta:globalLibVersion": "1.13.3"
    },
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:containerId": "tenant",
    "meta:tenantNamespace": "{TENANT_ID}"
}
```

## ターゲットデータセットの作成

[ カタログサービス API](https://www.adobe.io/experience-platform-apis/references/catalog/) に対してPOSTリクエストを実行し、ペイロード内のターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

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
        "name": "Target dataset for cloud storage",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/995dabbea86d58e346ff91bd8aa741a9f36f29b1019138d4",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `schemaRef.id` | ターゲット XDM スキーマの ID。 |
| `schemaRef.contentType` | スキーマのバージョン。 この値は `application/vnd.adobe.xed-full-notext+json;version=1` に設定する必要があります。これにより、スキーマの最新のマイナーバージョンが返されます。 |

**応答**

正常な応答は、新しく作成されたデータセットの ID を `"@/datasets/{DATASET_ID}"` の形式で含む配列を返します。 データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。ターゲットデータセット ID は、後の手順で、ターゲット接続とデータフローを作成する際に必要になります。

```json
[
    "@/dataSets/5f3c3cedb2805c194ff0b69a"
]
```

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取得されたデータの宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様 ID を指定する必要があります。 この接続仕様 ID は次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、ターゲットスキーマ、ターゲットデータセット、およびデータレイクへの接続仕様 ID の一意の識別子が得られました。 これらの識別子を使用して、[!DNL Flow Service] API を使用してターゲット接続を作成し、受信ソースデータを格納するデータセットを指定できます。

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
| `data.schema.version` | スキーマのバージョン。 この値は `application/vnd.adobe.xed-full+json;version=1` に設定する必要があります。これにより、スキーマの最新のマイナーバージョンが返されます。 |
| `params.dataSetId` | ターゲットデータセットの ID。 |
| `connectionSpec.id` | データレイクへの固定接続仕様 ID。 この ID は次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |

**応答**

正常な応答は、新しいターゲット接続の一意の識別子 (`id`) を返します。 この ID は後の手順で必要になります。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。 これは、リクエストペイロード内で定義されたPOSTマッピングを使用して、Conversion Service に対してデータリクエストを実行することで実現されます。

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

正常な応答は、新しく作成されたマッピングの詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この値は、後の手順でデータフローを作成する際に必要です。

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

データフローは、ソースからデータを収集し、それらを Platform に取り込みます。 データフローを作成するには、まずクラウドストレージデータの収集を担当するデータフロー仕様を取得する必要があります。

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

正常な応答は、ソースから Platform にデータを取り込む必要があるデータフロー仕様の詳細を返します。 応答には、新しいデータフローの作成に必要な固有のフロー仕様 `id` が含まれます。

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

クラウドストレージデータを収集する最後の手順は、データフローを作成することです。 現時点では、次の必須値が用意されています。

- [ソース接続 ID](#source)
- [ターゲット接続 ID](#target)
- [マッピング ID](#mapping)
- [データフロー仕様 ID](#specs)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。 データフローを作成するには、前述の値をPOST内に指定しながらペイロードリクエストを実行します。

取り込みをスケジュールするには、まず開始時間の値を秒単位のエポック時間に設定する必要があります。 次に、頻度の値を次の 5 つのオプションのいずれかに設定する必要があります。`once`、`minute`、`hour`、`day`、または `week`。 interval 値は、2 つの連続した取り込みから 1 回限りの取り込みを作成するまでの間隔を指定し、間隔を設定する必要はありません。 その他のすべての周波数の間隔値は、`15` 以上に設定する必要があります。

>[!IMPORTANT]
>
>[FTP コネクタ ](../../../connectors/cloud-storage/ftp.md) を使用する場合は、1 回限りの取り込みでデータフローをスケジュールすることを強くお勧めします。

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
| `flowSpec.id` | 前の手順で取得した [ フロー仕様 ID](#specs)。 |
| `sourceConnectionIds` | 前の手順で取得した [ ソース接続 ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した [ ターゲット接続 ID](#target-connection)。 |
| `transformations.params.mappingId` | 前の手順で取得した [ マッピング ID](#mapping)。 |
| `scheduleParams.startTime` | エポック時間でのデータフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。 指定できる値は次のとおりです。`once`、`minute`、`hour`、`day`、または `week`。 |
| `scheduleParams.interval` | この間隔は、2 つの連続したフロー実行の間隔を指定します。 間隔の値はゼロ以外の整数にする必要があります。 頻度が `once` に設定されている場合は、間隔は不要で、他の頻度値の場合は `15` 以上にする必要があります。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローの ID(`id`) が返されます。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## データフローの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。 データフローの監視方法の詳細については、API](../monitor.md) での [ データフローの監視に関するチュートリアルを参照してください。

## 次の手順

このチュートリアルでは、クラウドストレージからデータを収集するソースコネクタをスケジュールに従って作成しました。 受信データは、[!DNL Real-time Customer Profile] や [!DNL Data Science Workspace] など、ダウンストリームの Platform サービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

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
| [!DNL Azure Data Lake Storage Gen2] (ADLS Gen2) | `0ed90a81-07f4-4586-8190-b40eccef1c5a` |
| [!DNL Azure Event Hubs] （イベントハブ） | `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |
| [!DNL Azure File Storage] | `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8` |
| [!DNL Google Cloud Storage] | `32e8f412-cdf7-464c-9885-78184cb113fd` |
| [!DNL HDFS] | `54e221aa-d342-4707-bcff-7a4bceef0001` |
| [!DNL Oracle Object Storage] | `c85f9425-fb21-426c-ad0b-405e9bd8a46c` |
| [!DNL SFTP] | `bf367b0d-3d9b-4060-b67b-0d3d9bd06094` |
