---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;enable dataset
title: API を使用したプロファイルおよび ID サービスのデータセットの設定
topic: tutorial
type: Tutorial
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 56%

---


# APIのデータセットの設定 [!DNL Profile] と [!DNL Identity Service] 使用

This tutorial covers the process of enabling a dataset for use in [!DNL Real-time Customer Profile] and [!DNL Identity Service], broken down into the following steps:

1. Enable a dataset for use in [!DNL Real-time Customer Profile], using one of two options:
   - [新しいデータセットの作成](#create-a-dataset-enabled-for-profile-and-identity)
   - [既存のデータセットの設定](#configure-an-existing-dataset)
1. [データセットへのデータの取り込み](#ingest-data-into-the-dataset)
1. [リアルタイム顧客プロファイルによるデータ取り込みの確認](#confirm-data-ingest-by-real-time-customer-profile)
1. [ID サービスによるデータ取り込みの確認](#confirm-data-ingest-by-identity-service)

## はじめに

This tutorial requires a working understanding of the various Adobe Experience Platform services involved in managing [!DNL Profile]-enabled datasets. Before beginning this tutorial, please review the documentation for these related [!DNL Platform] services:

- [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [[!DNL Identity Service]](../../identity-service/home.md):取り込ま [!DNL Real-time Customer Profile] れる異なるデータソースのIDをブリッジ化することで有効に [!DNL Platform]します。
- [[!DNL Catalog Service]](../../catalog/home.md):および用のデータセットの作成と設定を可能にするRESTful API [!DNL Real-time Customer Profile] で [!DNL Identity Service]す。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

以下の節では、Platform API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in. For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

- x-sandbox-name: `{SANDBOX_NAME}`

## 有効にするデータセットの作成( [!DNL Profile] および [!DNL Identity] {#create-a-dataset-enabled-for-profile-and-identity}

You can enable a dataset for [!DNL Real-time Customer Profile] and [!DNL Identity Service] immediately upon creation or at any point after the dataset has been created. 作成済みのデータセットを有効にする場合は、このドキュメントの後半の手順に従って、[既存のデータセットを設定](#configure-an-existing-dataset)します。新しいデータセットを作成するには、リアルタイム顧客プロファイルが有効となっている既存の XDM スキーマの ID を知っておく必要があります。プロファイルが有効なスキーマを参照または作成する方法について詳しくは、[スキーマレジストリ API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを参照してください。次の [!DNL Catalog] APIの呼び出しは、およびのデータセットを有効に [!DNL Profile] し [!DNL Identity Service]ます。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

By including `unifiedProfile` and `unifiedIdentity` under `tags` in the request body, the dataset will be immediately enabled for [!DNL Profile] and [!DNL Identity Service], respectively. これらのタグの値は、`"enabled:true"` 文字列を含む配列である必要があります。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "fileDescription" : {
    "persisted": true,
        "containerFormat": "parquet",
        "format": "parquet"
    },
    "fields":[],
    "schemaRef" : {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/31670881463308a46f7d2cb09762715",
        "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
    },
    "tags" : {
       "unifiedProfile": ["enabled:true"],
       "unifiedIdentity": ["enabled:true"]
    }
  }'
```

| プロパティ | 説明 |
|---|---|
| `schemaRef.id` | The ID of the [!DNL Profile]-enabled schema upon which the dataset will be based. |
| `{TENANT_ID}` | The namespace within the [!DNL Schema Registry] which contains resources belonging to your IMS Organization. See the [TENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) section of the [!DNL Schema Registry] developer guide for more information. |

**応答** 

正常に完了すると、新しく作成されたデータセットの ID を含む配列が、`"@/dataSets/{DATASET_ID}"`の形式で表示されます。データセットの作成と有効化が完了したら、[データのアップロード](#upload-data-to-the-dataset)手順に進んでください。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

以下の手順では、 [!DNL Real-time Customer Profile] およびに対して以前に作成したデータセットを有効にする方法について説明 [!DNL Identity Service]します。 既にプロファイル対応データセットを作成している場合は、[データの取り込み](#ingest-data-into-the-dataset)手順に進んでください。

### データセットが有効かどうかを確認します。{#check-if-the-dataset-is-enabled}

Using the [!DNL Catalog] API, you can inspect an existing dataset to determine whether it is enabled for use in [!DNL Real-time Customer Profile] and [!DNL Identity Service]. 次の呼び出しは、データセットの詳細を ID によって取得します。

**API 形式**

```http
GET /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 調査するデータセットの ID。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{
    "5b020a27e7040801dedbf46e": {
        "name": "Commission Program Events DataSet",
        "imsOrg": "{IMS_ORG}",
        "tags": {
            "adobe/pqs/table": [
                "unifiedprofileingestiontesteventsdataset"
            ],
            "unifiedProfile": [
                "enabled:true"
            ],
            "unifiedIdentity": [
                "enabled:true"
            ]
        },
        "lastBatchId": "6dcd9128a1c84e6aa5177641165e18e4",
        "lastBatchStatus": "success",
        "dule": {},
        "statsCache": {
            "startDate": null,
            "endDate": null
        },
        "namespace": "ACP",
        "state": "DRAFT",
        "version": "1.0.1",
        "created": 1536536917382,
        "updated": 1539793978215,
        "createdClient": "{CLIENT_CREATED}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "viewId": "5b020a27e7040801dedbf46f",
        "status": "enabled",
        "fileDescription": {
            "persisted": true,
            "containerFormat": "parquet",
            "format": "parquet"
        },
        "transforms": "@/dataSets/5b020a27e7040801dedbf46e/views/5b020a27e7040801dedbf46f/transforms",
        "files": "@/dataSets/5b020a27e7040801dedbf46e/views/5b020a27e7040801dedbf46f/files",
        "schema": "@/xdms/context/experienceevent",
        "schemaMetadata": {
            "primaryKey": [],
            "delta": [],
            "dule": [],
            "gdpr": []
        },
        "schemaRef": {
            "id": "https://ns.adobe.com/xdm/context/experienceevent",
            "contentType": "application/vnd.adobe.xed+json"
        }
    }
}
```

プロパティ `tags` の下で、`unifiedProfile` と `unifiedIdentity` が値 `enabled:true` と共に存在することを確認できます。したがって、 [!DNL Real-time Customer Profile][!DNL Identity Service] とは、それぞれこのデータセットに対して有効になります。

### データセットの有効化 {#enable-the-dataset}

If the existing dataset has not been enabled for [!DNL Profile] or [!DNL Identity Service], you can enable it by making a PATCH request using the dataset ID.

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "tags" : {
        "unifiedProfile": ["enabled:true"],
        "unifiedIdentity": ["enabled:true"]
    }
  }'
```

リクエスト本文には、2 つのサブプロパティ（`"unifiedProfile"` および `"unifiedIdentity"`）を含む `tags` プロパティが含まれます。これらのサブプロパティの値は、`"enabled:true"` 文字列を含む配列です。

**応答** PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。`"unifiedProfile"` タグと `"unifiedIdentity"` タグが追加され、データセットがプロファイルおよび ID サービスで使用できるようになりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## データセットへのデータの取り込み {#ingest-data-into-the-dataset}

とは、XDMデータ [!DNL Real-time Customer Profile] がデータセットに取り込まれる際に、その両方を [!DNL Identity Service] 利用します。 データセットにデータをアップロードする手順については、[API を使用したデータセットの作成](../../catalog/datasets/create.md)に関するチュートリアルを参照してください。When planning what data to send to your [!DNL Profile]-enabled dataset, consider the following best practices:

- オーディエンスセグメント条件として使用するデータを含めます。
- ID グラフを最大化するために、プロファイルデータから確認できる識別子をできるだけ多く含めます。This allows [!DNL Identity Service] to stitch identities across datasets more effectively.

## データ取り込みの確認 [!DNL Real-time Customer Profile] {#confirm-data-ingest-by-real-time-customer-profile}

初めて新しいデータセットにデータをアップロードする際、または新しい ETL やデータソースが関与するプロセスの一部として、データが期待どおりにアップロードされたかどうかを慎重に確認することをお勧めします。Using the [!DNL Real-time Customer Profile] Access API, you can retrieve batch data as it gets loaded into a dataset. If you are unable to retrieve any of the entities you expect, your dataset may not be enabled for [!DNL Real-time Customer Profile]. データセットが有効になっていることを確認した後、ソースデータの形式と識別子が期待通りに動作することを確認します。APIを使用して [!DNL Real-time Customer Profile] データにアクセスする方法について詳しくは、「 [!DNL Profile] API」とも呼ばれる [エンティティのエンドポイントガイド](../api/entities.md)[!DNL Profile Access] に従ってください。

## ID サービスによるデータ取り込みの確認 {#confirm-data-ingest-by-identity-service}

複数の ID を含むデータフラグメントを取り込むと、プライベート ID グラフにリンクが作成されます。ID グラフと ID データへのアクセスの詳細については、[ID サービスの概要](../../identity-service/home.md)を参照してください。