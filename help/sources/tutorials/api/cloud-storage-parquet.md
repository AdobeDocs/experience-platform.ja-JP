---
keywords: Experience Platform；ホーム；人気のあるトピック；データソース接続
solution: Experience Platform
title: フローサービスAPIを使用したサードパーティのクラウドストレージシステムからのParquetデータの取り込み
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、フローサービスAPIを使用して、サードパーティのクラウドストレージシステムからApache Parquetデータを取り込む手順を説明します。
exl-id: fb1b19d6-16bb-4a5f-9e81-f537bac95041
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 24%

---

# [!DNL Flow Service] APIを使用して、サードパーティのクラウドストレージシステムからParquetデータを取り込む

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、サードパーティのクラウドストレージシステムからParquetデータを取り込む手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [ソース](../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
- [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してサードパーティのクラウドストレージからParquetデータを正しく取り込むために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

- `Content-Type: application/json`

## 接続の作成

[!DNL Platform] APIを使用してParquetデータを取り込むには、アクセスするサードパーティのクラウドストレージソースに対して有効な接続が必要です。 使用するストレージの接続がまだない場合は、次のチュートリアルを通じてストレージを作成できます。

- [Amazon S3](./create/cloud-storage/s3.md)
- [Azure BLOB](./create/cloud-storage/blob.md)
- [Azure Data Lake Storage Gen2](./create/cloud-storage/adls-gen2.md)
- [Google Cloud Store](./create/cloud-storage/google.md)
- [SFTP](./create/cloud-storage/sftp.md)

接続の一意の識別子(`$id`)を取得して保存し、このチュートリアルの次の手順に進みます。

## ターゲットスキーマの作成

[!DNL Platform]でソースデータを使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマも作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが格納される[!DNL Platform]データセットを作成します。

[!DNL Experience Platform]でユーザーインターフェイスを使用したい場合は、[スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)で、スキーマエディターで同様の操作を実行する手順を順を追って説明します。

**API 形式**

```http
POST /schemaregistry/tenant/schemas
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
    "title": "Sample Demo Profile XDM {{$guid}}",
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
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-work-details"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-subscriptions"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/identitymap"
        }
    ],
    "meta:containerId": "tenant",
    "meta:resourceType": "schemas",
    "meta:xdmType": "object",
    "meta:class": "https://ns.adobe.com/xdm/context/profile"
}'
```

**応答**

正常な応答は、新しく作成されたスキーマの一意の識別子(`$id`)を含む詳細を返します。 このIDは、次の手順でソース接続を作成する際に必要になります。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/e15530faf88aeb52d9ca5c5671a059f44f1a42ea7f5fdb80",
    "meta:altId": "_{TENANT_ID}.schemas.e15530faf88aeb52d9ca5c5671a059f44f1a42ea7f5fdb80",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "Sample Demo Profile XDM 8d96a964-aad8-43c5-a73a-c8b9b1ccbfb1",
    "type": "object",
    "description": "",
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
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-work-details",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-subscriptions",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/identitymap",
            "type": "object",
            "meta:xdmType": "object"
        }
    ],
    "refs": [
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/context/profile-subscriptions",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/context/profile-work-details"
    ],
    "imsOrg": "{IMS_ORG}",
    "meta:extensible": false,
    "meta:abstract": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/context/profile-subscriptions",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/context/profile-work-details"
    ],
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1584673864341,
        "repo:lastModifiedDate": 1584673864341,
        "xdm:createdClientId": "{CREATED_CLIENT_ID}",
        "xdm:lastModifiedClientId": "{MODIFIED_CLIENT_ID}",
        "xdm:createdUserId": "{CREATED_USER_ID}",
        "xdm:lastModifiedUserId": "{MODIFIED_USER_ID}",
        "eTag": "fa704f80da907c8f0f66f453ffcac3e52958687edbf55d71231dc5e1522193c4"
    },
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:containerId": "tenant",
    "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## ソース接続の作成 {#source}

ターゲットXDMスキーマを作成したら、[!DNL Flow Service] APIへのPOSTリクエストを使用してソース接続を作成できるようになりました。 ソース接続は、APIの接続、ソースデータ形式、前の手順で取得したターゲットXDMスキーマへの参照で構成されます。

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Source Connection S3 {{$guid}}",
        "baseConnectionId": "5831c52c-c261-4945-b1c5-2cc261d945b2",
        "connectionSpec": {
            "id": "ecadc60c-7455-4d87-84dc-2a0e293d997b",
            "version": 1
        },
        "data": {
            "format": "parquet_xdm",
            "schema": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/e15530faf88aeb52d9ca5c5671a059f44f1a42ea7f5fdb80",
                "id": "",
                "version": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "params": {
            "path": "partners-demo/samples",
            "recursive": "true"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `baseConnectionId` | クラウドストレージを表すAPIの接続。 |
| `data.schema.id` | (`$id`)（前の手順で取得したターゲットxdmスキーマの場合）。 |
| `params.path` | ソースファイルのパス。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子(`id`)を返します。 この値は、後の手順でターゲット接続を作成する際に必要になるので保存します。

```json
{
    "id": "73bc8911-505a-4e46-bc89-11505a6e466f",
    "etag": "\"c4004435-0000-0200-0000-5e7437d90000\""
}
```

## データセットベース接続の作成

外部データを[!DNL Platform]に取り込むには、まず[!DNL Experience Platform]データセットのベース接続を取得する必要があります。

データセットベースの接続を作成するには、[データセットベースの接続に関するチュートリアル](./create-dataset-base-connection.md)で説明している手順に従います。

データセットベースの接続を作成するまで、開発者ガイドで説明されている手順に従います。 一意の識別子(`$id`)を取得して保存し、次の手順でターゲット接続を作成する際に、ベース接続IDとして使用します。

## ターゲットデータセットの作成

[カタログサービスAPI](https://www.adobe.io/experience-platform-apis/references/catalog/)に対してPOSTリクエストを実行し、ペイロード内のターゲットスキーマのIDを指定することで、ターゲットデータセットを作成できます。

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
        "name": "Leads Dataset {{$guid}}",
        "schemaRef": {
            "id": ""https://ns.adobe.com/{TENANT_ID}/schemas/e15530faf88aeb52d9ca5c5671a059f44f1a42ea7f5fdb80"",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef.id` | ターゲットXDMスキーマのID。 |

**応答**

正常な応答は、新しく作成されたデータセットのIDを`"@/datasets/{DATASET_ID}"`の形式で含む配列を返します。 データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。後の手順でターゲット接続とデータフローを作成する際に必要になるターゲットデータセットIDを保存します。

```json
[
    "@/dataSets/5e7439b1ad55a618ad4c5102"
]
```

## ターゲット接続の作成 {#target}

データセットベース接続、ターゲットスキーマ、ターゲットデータセットに固有の識別子が割り当てられました。 これらの識別子を使用して、[!DNL Flow Service] APIを使用してターゲット接続を作成し、受信ソースデータを格納するデータセットを指定できます。

**API 形式**

```http
POST /targetConnections
```

**リクエスト**

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/targetConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "baseConnectionId": "291257e3-c560-4e07-9257-e3c5606e07d1",
        "connectionSpec": {
            "id":"c604ff05-7f1a-43c0-8e18-33bf874cb11c",
            "version": "1.0"
        },
        "name": "Target Connection {{$guid}}",
        "data": {
            "format": "parquet_xdm",
            "schema": {
                "id": ""https://ns.adobe.com/{TENANT_ID}/schemas/e15530faf88aeb52d9ca5c5671a059f44f1a42ea7f5fdb80"",
                "version": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "params": {
            "dataSetId": "5e7439b1ad55a618ad4c5102"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `baseConnectionId` | データセットベース接続のID。 |
| `data.schema.id` | ターゲットXDMスキーマの`$id`。 |
| `params.dataSetId` | ターゲットデータセットのID。 |
| `connectionSpec.id` | クラウドストレージの接続仕様ID。 |

**応答**

正常な応答は、新しいターゲット接続の一意の識別子(`id`)を返します。 この値は、後の手順で必要になるため保存します。

```json
{
    "id": "9b3abc95-f2e9-47c1-babc-95f2e927c1ec",
    "etag": "\"7501936b-0000-0200-0000-5e743bcc0000\""
}
```

## データフローの作成

サードパーティのクラウドストレージからParquetデータを取り込む最後の手順は、データフローを作成することです。 現時点では、次の必要な値が用意されています。

- [ソース接続ID](#source)
- [ターゲット接続ID](#target)

データフローは、ソースからデータをスケジュールおよび収集する役割を担います。 データフローを作成するには、前述の値をPOST内に提供しながらペイロードリクエストを実行します。

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
        "name": "Demo Parquet Ingestion Flow {{$guid}}",
        "flowSpec": {
            "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "73bc8911-505a-4e46-bc89-11505a6e466f"
        ],
        "targetConnectionIds": [
            "9b3abc95-f2e9-47c1-babc-95f2e927c1ec"
        ],
        "scheduleParams": {
            "startTime": {{$timestamp}},
            "frequency": "minute",
            "interval": 1000,
            "backfill": true
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sourceConnectionIds` | 前の手順で取得したソース接続ID。 |
| `targetConnectionIds` | 前の手順で取得したターゲット接続ID。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローのID(`id`)が返されます。

```json
{
    "id": "89ff50ef-b082-426e-bf50-efb082d26e78",
    "etag": "\"890070b8-0000-0200-0000-5e743c040000\""
}
```

## 次の手順

このチュートリアルに従って、ソースコネクタを作成し、サードパーティのクラウドストレージシステムからParquetデータをスケジュールに従って収集しました。 受信データは、[!DNL Real-time Customer Profile]や[!DNL Data Science Workspace]など、ダウンストリームの[!DNL Platform]サービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../profile/home.md)
- [Data Science Workspace の概要](../../../data-science-workspace/home.md)
