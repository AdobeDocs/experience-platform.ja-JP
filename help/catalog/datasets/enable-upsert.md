---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、データセットの有効化
title: API を使用したプロファイル更新のデータセットの有効化
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform API を使用して、リアルタイム顧客プロファイルデータを更新するための「アップサート」機能を持つデータセットを有効にする方法について説明します。
exl-id: fc89bc0a-40c9-4079-8bfc-62ec4da4d16a
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 35%

---

# API を使用したプロファイル更新のデータセットの有効化

このチュートリアルでは、リアルタイム顧客プロファイルデータを更新するために、「アップサート」機能を持つデータセットを有効にするプロセスについて説明します。 これには、新しいデータセットを作成し、既存のデータセットを設定する手順が含まれます。

## はじめに

このチュートリアルでは、プロファイル対応データセットの管理に関わるいくつかのAdobe Experience Platformサービスに関する十分な知識が必要です。 このチュートリアルを開始する前に、次の関連する DNL Platform サービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Catalog Service]](../../catalog/home.md):データセットを作成し、それらを設定できる RESTful API [!DNL Real-time Customer Profile] および [!DNL Identity Service].
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

ペイロード (POST、PUT、PATCH) を含むすべてのリクエストには、追加の `Content-Type` ヘッダー。 必要に応じて、このヘッダーの正しい値がサンプルリクエストに表示されます。

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。へのすべてのリクエスト [!DNL Platform] API には `x-sandbox-name` 操作がおこなわれるサンドボックスの名前を指定するヘッダー。 [!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## プロファイル更新対応データセットの作成

新しいデータセットを作成する際に、そのデータセットのプロファイルを有効にし、作成時に更新機能を有効にすることができます。

>[!NOTE]
>
>新しいプロファイル対応データセットを作成するには、プロファイルに対して有効になっている既存の XDM スキーマの ID を知っておく必要があります。 プロファイルが有効なスキーマを参照または作成する方法について詳しくは、[スキーマレジストリ API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを参照してください。

プロファイルと更新が有効なデータセットを作成するには、 `/dataSets` endpoint.

**API 形式**

```http
POST /dataSets
```

**リクエスト**

次を含めることで： `unifiedProfile` under `tags` リクエスト本文では、データセットは [!DNL Profile] 作成時に 内 `unifiedProfile` 配列、追加 `isUpsert:true` は、更新をサポートするデータセットの機能を追加します。

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
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/31670881463308a46f7d2cb09762715",
          "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        },
        "tags": {
          "unifiedProfile": [
            "enabled:true",
            "isUpsert:true"
          ]
        }
      }'
```

| プロパティ | 説明 |
|---|---|
| `schemaRef.id` | の ID [!DNL Profile] — データセットの基となる有効なスキーマ。 |
| `{TENANT_ID}` | 内の名前空間 [!DNL Schema Registry] :IMS 組織に属するリソースを含みます。 詳しくは、 [TENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) セクション [!DNL Schema Registry] 開発者ガイドを参照してください。 |

**応答**

正常に完了すると、新しく作成されたデータセットの ID を含む配列が、`"@/dataSets/{DATASET_ID}"`の形式で表示されます。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

次の手順では、更新（「アップサート」）機能用に既存のプロファイル対応データセットを設定する方法を説明します。

>[!NOTE]
>
>既存のプロファイル対応データセットを「アップサート」用に設定するには、まずプロファイルのデータセットを無効にし、次に `isUpsert` タグを使用します。 既存のデータセットのプロファイルが有効になっていない場合は、次の手順に直接進むことができます： [プロファイルのデータセットの有効化とアップサート](#enable-the-dataset). 不明な場合は、次の手順で、データセットが既に有効になっているかどうかを確認する方法を示します。

### データセットのプロファイルが有効かどうかを確認します

の使用 [!DNL Catalog] API を使用すると、既存のデータセットを調べて、そのデータセットがでの使用に対して有効になっているかどうかを判断できます [!DNL Real-time Customer Profile]. 次の呼び出しは、データセットの詳細を ID によって取得します。

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

以下 `tags` プロパティは、 `unifiedProfile` が値と共に存在する `enabled:true`. したがって [!DNL Real-time Customer Profile] は、このデータセットに対して有効になっています。

### プロファイルのデータセットを無効にする

プロファイル対応のデータセットを更新用に設定するには、まず `unifiedProfile` タグ付けし、同時に再度有効にする `isUpsert` タグを使用します。 これは、2 つのPATCHリクエストを使用しておこなわれます。1 回は無効にし、もう 1 回は再度有効にします。

>[!WARNING]
>
>無効になっている間にデータセットに取り込まれたデータは、プロファイルストアに取り込まれません。 プロファイルに対して再度有効になるまで、データセットにデータを取り込まないようにすることをお勧めします。

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

最初のPATCHリクエスト本文には、 `path` から `unifiedProfile` 設定 `value` から `enabled:false` タグを無効にするために使用します。

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

**応答** PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。この `unifiedProfile` タグが無効になりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

### プロファイルのデータセットを有効にしてアップサート {#enable-the-dataset}

既存のデータセットは、1 回のPATCHリクエストで、プロファイルと属性の更新に対して有効にできます。

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

リクエスト本文には、 `path` から `unifiedProfile` 設定 `value` を含めるには `enabled` および `isUpsert` タグ、両方ともに設定 `true`.

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

**応答** PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。この `unifiedProfile` タグが有効になり、属性の更新用に設定されました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## 次の手順

これで、プロファイルとアップサートが有効なデータセットを、バッチおよびストリーミングの取り込みワークフローで使用して、プロファイルデータを更新できるようになりました。 データをAdobe Experience Platformに取り込む方法の詳細については、まず [データ取得の概要](../../ingestion/home.md).
