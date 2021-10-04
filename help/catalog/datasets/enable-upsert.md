---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、データセットの有効化
title: API を使用したプロファイル更新データセットの有効化
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform API を使用して、リアルタイム顧客プロファイルデータを更新するための「upsert」機能を持つデータセットを有効にする方法について説明します。
exl-id: fc89bc0a-40c9-4079-8bfc-62ec4da4d16a
source-git-commit: 648923a0a124767f530bea09519449f76d576b5e
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 35%

---

# API を使用したプロファイル更新のデータセットの有効化

このチュートリアルでは、リアルタイム顧客プロファイルデータを更新するために、「upsert」機能を持つデータセットを有効にするプロセスについて説明します。 新しいデータセットの作成手順や、既存のデータセットの設定手順も含まれます。

## はじめに

このチュートリアルでは、プロファイル対応データセットの管理に関わるAdobe Experience Platformの複数のサービスに関する十分な知識が必要です。 このチュートリアルを開始する前に、次の関連する DNL Platform サービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Catalog Service]](../../catalog/home.md):データセットを作成し、および用に設定できる RESTful API [!DNL Real-time Customer Profile] で [!DNL Identity Service]す。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。
- [バッチ取得](../../ingestion/batch-ingestion/overview.md)

以下の節では、Platform API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

ペイロード (POST、PUT、PATCH) を含むすべてのリクエストには、追加の `Content-Type` ヘッダーが必要です。 必要に応じて、このヘッダーの正しい値がサンプルリクエストに表示されます。

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定する `x-sandbox-name` ヘッダーが必要です。 [!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## プロファイル更新対応データセットの作成

新しいデータセットを作成する際に、そのデータセットをプロファイルに対して有効にし、作成時に更新機能を有効にすることができます。

>[!NOTE]
>
>新しいプロファイル対応データセットを作成するには、プロファイルに対して有効になっている既存の XDM スキーマの ID を把握しておく必要があります。 プロファイルが有効なスキーマを参照または作成する方法について詳しくは、[スキーマレジストリ API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを参照してください。

プロファイルと更新が有効なデータセットを作成するには、`/dataSets` エンドポイントへのPOSTリクエストを使用します。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

リクエスト本文の `tags` の下に `unifiedProfile` を含めると、データセットの作成時に [!DNL Profile] が有効になります。 `unifiedProfile` 配列内に `isUpsert:true` を追加すると、更新をサポートするデータセットの機能が追加されます。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "fields":[],
        "schemaRef" : {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/31670881463308a46f7d2cb09762715",
          "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        },
        "tags" : {
          "unifiedProfile": [
            "enabled:true",
            "isUpsert:true"
          ]
        }
      }'
```

| プロパティ | 説明 |
|---|---|
| `schemaRef.id` | データセットの基となる [!DNL Profile] 対応スキーマの ID。 |
| `{TENANT_ID}` | [!DNL Schema Registry] 内の名前空間。IMS 組織に属するリソースが含まれます。 詳しくは、[!DNL Schema Registry] 開発者ガイドの [TENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) の節を参照してください。 |

**応答**

正常に完了すると、新しく作成されたデータセットの ID を含む配列が、`"@/dataSets/{DATASET_ID}"`の形式で表示されます。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

次の手順では、更新 (「upsert」) 機能用に既存のプロファイル対応データセットを設定する方法を説明します。

>[!NOTE]
>
>既存のプロファイル対応データセットを「upsert」用に設定するには、まずプロファイルのデータセットを無効にしてから、`isUpsert` タグと共に再度有効にする必要があります。 既存のデータセットがプロファイルに対して有効になっていない場合は、[ プロファイルのデータセットを有効にし、](#enable-the-dataset) を挿入する手順に直接進むことができます。 不明な場合は、次の手順で、データセットが既に有効になっているかどうかを確認します。

### データセットのプロファイルが有効かどうかの確認

[!DNL Catalog] API を使用して、既存のデータセットを調べ、[!DNL Real-time Customer Profile] での使用が有効になっているかどうかを判断できます。 次の呼び出しは、データセットの詳細を ID によって取得します。

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
        "name": "{DATASET_NAME}",
        "imsOrg": "{IMS_ORG}",
        "tags": {
            "adobe/pqs/table": [
                "unifiedprofileingestiontesteventsdataset"
            ],
            "unifiedProfile": [
                "enabled:true"
            ]
        },
        "lastBatchId": "{BATCH_ID}",
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
        "viewId": "{VIEW_ID}",
        "status": "enabled",
        "transforms": "@/dataSets/5b020a27e7040801dedbf46e/views/5b020a27e7040801dedbf46f/transforms",
        "files": "@/dataSets/5b020a27e7040801dedbf46e/views/5b020a27e7040801dedbf46f/files",
        "schema": "{SCHEMA}",
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

`tags` プロパティの下で、`unifiedProfile` が値 `enabled:true` と共に存在することがわかります。 したがって、このデータセットでは [!DNL Real-time Customer Profile] が有効になっています。

### プロファイルのデータセットの無効化

プロファイル対応データセットを更新用に設定するには、まず `unifiedProfile` タグを無効にし、次に `isUpsert` タグと共に再度有効にする必要があります。 これは、2 つのPATCHリクエストを使用しておこなわれます。1 回は無効にし、もう 1 回は再度有効にします。

>[!WARNING]
>
>無効な状態でデータセットに取り込まれたデータは、プロファイルストアに取り込まれません。 プロファイルに対して再度有効になるまで、データセットにデータを取り込まないようにすることをお勧めします。

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

第 1 のPATCH要求本体は、タグを無効にするために `value` を `enabled:false` に設定する `path` から `unifiedProfile` を含む。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e \
  -H 'Content-Type:application/json-patch+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "replace", "path": "/tags/unifiedProfile", "value": ["enabled:false"] },
      ]'
```

**応答** PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。`unifiedProfile` タグは無効になりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

### プロファイル用のデータセットの有効化とアップサート {#enable-the-dataset}

既存のデータセットは、1 回のプロファイルリクエストを使用して、プロファイルと属性の更新に対して有効にすることがPATCHできます。

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

リクエスト本文には、`enabled` タグと `isUpsert` タグを含むように `value` を設定する `path` ～ `unifiedProfile` が含まれ、両方とも `true` に設定されます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e \
  -H 'Content-Type:application/json-patch+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/tags/unifiedProfile", "value": ["enabled:true","isUpsert:true"] },
      ]'
```

**応答** PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。`unifiedProfile` タグが有効になり、属性の更新が設定されました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## 次の手順

これで、プロファイルとアップサート対応のデータセットを、バッチおよびストリーミングの取り込みワークフローで使用して、プロファイルデータを更新できるようになりました。 データをAdobe Experience Platformに取り込む方法の詳細については、まず「[ データ取り込みの概要 ](../../ingestion/home.md)」を参照してください。
