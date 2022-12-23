---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API;データセットの有効化
title: API を使用したデータセットのプロファイル更新の有効化
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform API を使用して、リアルタイム顧客プロファイルデータを更新するための「アップサート」機能によってデータセットを有効にする方法について説明します。
exl-id: fc89bc0a-40c9-4079-8bfc-62ec4da4d16a
source-git-commit: 132407af947b97a1925799a1fb5e12caa2b0410c
workflow-type: ht
source-wordcount: '1050'
ht-degree: 100%

---

# API を使用したデータセットのプロファイル更新の有効化

このチュートリアルでは、リアルタイム顧客プロファイルデータを更新するために、「アップサート」機能によってデータセットを有効にするプロセスについて説明します。 これには、新しいデータセットを作成し、既存のデータセットを設定する手順が含まれます。

>[!NOTE]
>
>アップサートワークフローは、バッチ取り込みに対してのみ機能します。 ストリーミング取り込みはサポート&#x200B;**対象外**&#x200B;です。

## はじめに

このチュートリアルでは、プロファイル対応データセットに関連する様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、[!DNL Platform] サービスについての関連ドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Catalog Service]](../../catalog/home.md)：データセットを [!DNL Real-time Customer Profile] および [!DNL Identity Service] 用に作成し、設定できる RESTful API。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
- [バッチ取り込み](../../ingestion/batch-ingestion/overview.md)：Batch Ingestion API を使用して、データをバッチファイルとして Experience Platform に取り込むことができます。

以下の節では、Platform API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

ペイロード（POST、PUT、PATCH）を含んだすべてのリクエストには、追加の `Content-Type` ヘッダーが必要です。必要に応じて、このヘッダーの正しい値がサンプルリクエストに表示されます。

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定する `x-sandbox-name` ヘッダーが必要です。[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## プロファイル更新が有効なデータセットの作成

新しいデータセットを作成する際に、そのデータセットのプロファイルを有効にし、作成時に更新機能を有効にできます。

>[!NOTE]
>
>プロファイルを有効にしたデータセットを新規作成するには、プロファイルを有効にした既存 XDM スキーマの ID を知っておく必要があります。プロファイルが有効なスキーマを参照または作成する方法について詳しくは、[Schema Registry API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを参照してください。

プロファイルおよび更新を有効化したデータセットを作成するには、`/dataSets` エンドポイントに POST リクエストを使用します。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

リクエスト本文で、`tags` の下に `unifiedIdentity` と `unifiedProfile` の両方を含めることで、作成時に [!DNL Profile] を有効にしたデータセットになります。`unifiedProfile` 配列内で `isUpsert:true` を追加することで、データセットに更新をサポートする機能が追加されます。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "Sample dataset",
        "description: "A sample dataset with a sample description.",
        "fields": [],
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/31670881463308a46f7d2cb09762715",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
        },
        "tags": {
            "unifiedIdentity": [
                "enabled: true"
            ],
            "unifiedProfile": [
                "enabled: true",
                "isUpsert: true"
            ]
        }
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef.id` | データセットのベースとなる [!DNL Profile] 対応スキーマの ID。 |
| `{TENANT_ID}` | 組織に属するリソースを含む [!DNL Schema Registry] 内の名前空間。詳しくは、[!DNL Schema Registry] 開発者ガイドの [TENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) の節を参照してください。 |

**応答**

正常に完了すると、新しく作成されたデータセットの ID を含む配列が、`"@/dataSets/{DATASET_ID}"`の形式で表示されます。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

次の手順では、既存のプロファイルを有効にしたデータセットを更新（アップサート）機能用に設定する方法を説明します。

>[!NOTE]
>
>既存のプロファイルを有効にしたデータセットをアップサート用に設定するには、まずデータセットのプロファイル有効を無効にし、次に `isUpsert` タグと共に再度有効にします。 既存のデータセットがプロファイル対応になっていない場合は、[データセットのプロファイルとアップサート対応](#enable-the-dataset)手順に直接進みます。 不明な場合は、次の手順で、データセットが既に有効になっているかどうかを確認できます。

### データセットがプロファイルに対して有効かどうかを確認

[!DNL Catalog] API を使用すると、既存のデータセットを調べて、そのデータセットが [!DNL Real-time Customer Profile] での使用に対して有効になっているかどうかを判断できます。 次の呼び出しは、データセットの詳細を ID によって取得します。

**API 形式**

```http
GET /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 調査するデータセットの ID。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```json
{
    "5b020a27e7040801dedbf46e": {
        "name": "{DATASET_NAME}",
        "imsOrg": "{ORG_ID}",
        "tags": {
            "adobe/pqs/table": [
                "unifiedprofileingestiontesteventsdataset"
            ],
            "unifiedIdentity": [
                "enabled:true"
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
            "dule": []
        },
        "schemaRef": {
            "id": "https://ns.adobe.com/xdm/context/experienceevent",
            "contentType": "application/vnd.adobe.xed+json"
        }
    }
}
```

`tags` プロパティの下で、`unifiedProfile` の値が `enabled:true` となっていることがわかります。したがって、[!DNL Real-time Customer Profile] はこのデータセットに対して有効になっています。

### プロファイルのデータセットを無効にする

プロファイルが有効になっているデータセットを更新用に設定するには、まず `unifiedProfile` タグと `unifiedIdentity` タグを無効化してから、それらを `isUpsert` タグとともに再度有効にします。これを実行するには、2 つの PATCH リクエストを使用します。1 回目のリクエストで無効にし、もう 1 回で再度有効にします。

>[!WARNING]
>
>無効になっている間にデータセットに取り込まれたデータは、プロファイルストアに取り込まれません。 プロファイルに対してデータセットが再度有効になるまでは、データセットにデータを取り込まないようにする必要があります。

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

最初の PATCH リクエスト本文には、`unifiedProfile` への `path` および `unifiedIdentity` への `path` がが含まれ、タグを無効にするため、両方のパスで `value` を `enabled:false` に設定します。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e \
  -H 'Content-Type:application/json-patch+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { 
            "op": "replace", 
            "path": "/tags/unifiedProfile", 
            "value": ["enabled:false"] 
        },
        {
            "op": "replace",
            "path": "/tags/unifiedIdentity",
            "value": ["enabled:false"]
        }
      ]'
```

**応答**

応答 PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。`unifiedProfile` タグと `unifiedIdentity` タグが無効になりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

### プロファイルでデータセットを有効にしてアップサートする {#enable-the-dataset}

単一の PATCH リクエストを使用して、プロファイルと属性の更新に対して既存のデータセットを有効にできます。

>[!IMPORTANT]
>
>プロファイルに対してデータセットを有効にする場合は、データセットが関連付けられているスキーマ&#x200B;**も**&#x200B;プロファイルに対して有効になっている必要があります。スキーマがプロファイル対応でない場合、データセットは Platform UI 内でプロファイル対応として表示され&#x200B;**ません**。

**API 形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{DATASET_ID}` | 更新するデータセットの ID。 |

**リクエスト**

リクエスト本文には、`unifiedProfile` への `path`（`value` では `enabled` タグと `isUpsert` タグを含め、両方のタグを `true` にするよう設定）、および `unifiedIdentity` への `path`（`value` では `enabled` タグを含めて `true` にするよう設定）が含まれます。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/catalog/dataSets/5b020a27e7040801dedbf46e \
  -H 'Content-Type:application/json-patch+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { 
            "op": "add", 
            "path": "/tags/unifiedProfile", 
            "value": [
                "enabled:true",
                "isUpsert:true"
            ] 
        },
        {
            "op": "add",
            "path": "/tags/unifiedIdentity",
            "value": [
                "enabled:true"
            ]
        }
      ]'
```

**応答**

応答 PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。`unifiedProfile` タグと `unifiedIdentity` タグが有効になり、属性の更新用に設定されました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## 次の手順

これで、プロファイルとアップサートが有効になっているデータセットをバッチ取得ワークフローで使用して、プロファイルデータを更新できるようになりました。 データを Adobe Experience Platform に取り込む方法の詳細については、まず [データ取り込みの概要](../../ingestion/home.md)を参照してください。
