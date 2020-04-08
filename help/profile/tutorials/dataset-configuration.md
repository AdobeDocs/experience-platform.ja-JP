---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: APIを使用したプロファイルおよびIDサービスのデータセットの設定
topic: tutorial
translation-type: tm+mt
source-git-commit: 409d98818888f2758258441ea2d993ced48caf9a

---


# APIを使用したプロファイルおよびIDサービスのデータセットの設定

このチュートリアルでは、リアルタイム顧客プロファイルおよびIDサービスで使用するデータセットを有効にするプロセスを、次の手順に分けて説明します。

1. 次の2つのオプションのいずれかを使用して、リアルタイム顧客プロファイルで使用するデータセットを有効にします。
   - [新しいデータセットの作成](#create-a-dataset-enabled-for-profile-and-identity)
   - [既存のデータセットの設定](#configure-an-existing-dataset)
1. [データセットへのデータの取り込み](#ingest-data-into-the-dataset)
1. [リアルタイム顧客によるデータ取り込みの確認プロファイル](#confirm-data-ingest-by-real-time-customer-profile)
1. [IDサービスによるデータ取り込みの確認](#confirm-data-ingest-by-identity-service)

## はじめに

このチュートリアルでは、プロファイル対応データセットの管理に関わる様々なAdobe Experience Platformサービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、次の関連するプラットフォームサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
- [IDサービス](../../identity-service/home.md):異なるデータソースのIDをPlatformに取り込むことで、リアルタイムの顧客プロファイルを有効にします。
- [Catalog Service](../../catalog/home.md):データセットを作成し、リアルタイム顧客プロファイルとIDサービス用に設定できるRESTful APIです。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。 プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

- x-sandbox-name: `{SANDBOX_NAME}`

## データセットの作成(プロファイルとID) {#create-a-dataset-enabled-for-profile-and-identity}

リアルタイム顧客プロファイルおよびIDサービスのデータセットは、作成時に、またはデータセットの作成後任意の時点で有効にできます。 作成済みのデータセットを有効にする場合は、このドキュメントの後半の手順に従っ [て、既存のデータセット](#configure-an-existing-dataset) を設定します。 新しいデータセットを作成するには、リアルタイム顧客プロファイルが有効な既存のXDMスキーマのIDを知っている必要があります。 プロファイルが有効なスキーマを参照または作成する方法について詳しくは、スキーマレジストリAPIを使用した [スキーマの作成に関するチュートリアルを参照してください](../../xdm/tutorials/create-schema-api.md)。 次に示すCatalog APIの呼び出しにより、プロファイルとIDサービスのデータセットが有効になります。

**API形式**

```http
POST /dataSets
```

**リクエスト**

リクエスト本 `unifiedProfile` 文にと `unifiedIdentity` を含め `tags` ることで、データセットは、プロファイルとIDサービスに対して直ちに有効になります。 これらのタグの値は、文字列を含む配列である必要がありま `"enabled:true"`す。

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
| `schemaRef.id` | プロファイルセットの基となるスキーマ対応データセットのID。 |
| `{TENANT_ID}` | 名前空間レジストリ内のスキーマで、IMS組織に属するリソースが含まれます。 詳しくは、 [スキーマレジストリ開発者ガイドのTENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) の節を参照してください。 |

**応答**

正常に完了すると、新しく作成されたデータセットのIDを含む配列が、の形式で表示されま `"@/dataSets/{DATASET_ID}"`す。 データセットの作成と有効化が完了したら、データのアップロード手順に進 [んでください](#upload-data-to-the-dataset)。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

以下の手順では、以前に作成したデータセットをリアルタイム顧客プロファイルおよびIDサービス用に有効にする方法を説明します。 既にデータセットがプロファイル対応の場合は、データの取り込み手順に進ん [でください](#ingest-data-into-the-dataset)。

### データセットが有効かどうかを確認します。 {#check-if-the-dataset-is-enabled}

カタログAPIを使用すると、既存のデータセットを調べて、それがリアルタイム顧客プロファイルおよびIDサービスでの使用に対して有効になっているかどうかを判断できます。 次の呼び出しは、データセットの詳細をIDで取得します。

**API形式**

```http
GET /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 調査するデータセットのID。 |

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

プロパティ `tags` の下で、とが値と共に `unifiedProfile` 存在す `unifiedIdentity` ることを確認できます `enabled:true`。 したがって、このデータセットに対しては、Real-time CustomerプロファイルとIdentity Serviceがそれぞれ有効になります。

### データセットの有効化 {#enable-the-dataset}

既存のデータセットがプロファイルまたはIDサービスに対して有効になっていない場合は、データセットIDを使用してPATCHリクエストを作成することで有効にできます。

**API形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DATASET_ID}` | 更新するデータセットのID。 |

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

リクエスト本文には、次の2つ `tags` のサブプロパティを含むプロパティが含まれます。 `"unifiedProfile"` と `"unifiedIdentity"`これらのサブプロパティの値は、文字列を含む配列です `"enabled:true"`。

**応答** PATCHリクエストが成功すると、HTTP Status 200(OK)と、更新されたデータセットのIDを含む配列が返されます。 このIDは、PATCHリクエストで送信されたIDと一致する必要があります。 タグとタ `"unifiedProfile"` グが追 `"unifiedIdentity"` 加され、データセットがプロファイルおよびIDサービスで使用できるようになりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## データセットへのデータの取り込み {#ingest-data-into-the-dataset}

リアルタイム顧客プロファイルとIDサービスの両方が、データセットに取り込まれる際にXDMデータを使用します。 データセットにデータをアップロードする手順については、APIを使用したデータセットの作成に関するチ [ュートリアルを参照してください](../../catalog/datasets/create.md)。 プロファイル対応データセットに送信するデータを計画する際には、次のベストプラクティスを検討してください。

- データセグメント条件として使用するオーディエンスを含めます。
- 識別子のグラフを最大化するために、識別子をプロファイルデータから確認できるだけ含めます。 これにより、IDサービスは、データセット間でIDをより効果的に結合できます。

## リアルタイム顧客によるデータ取り込みの確認プロファイル {#confirm-data-ingest-by-real-time-customer-profile}

初めて新しいデータセットにデータをアップロードする場合、または新しいETLやデータソースが関与するプロセスの一部として、データが期待どおりにアップロードされたかどうかを慎重に確認することをお勧めします。 リアルタイム顧客プロファイルアクセスAPIを使用すると、データセットに読み込まれるバッチデータを取得できます。 期待するエンティティを取得できない場合は、データセットでリアルタイム顧客プロファイルが有効になっていない可能性があります。 データセットが有効になっていることを確認した後、ソースデータの形式と識別子が期待通りに動作することを確認します。 リアルタイム顧客プロファイルAPIを使用してプロファイルデータにアクセスする方法について詳しくは、「プロファイルアクセスAPI」とも呼ばれるエンティティのサブガ [イドに従ってください](../api/entities.md)。

## IDサービスによるデータ取り込みの確認 {#confirm-data-ingest-by-identity-service}

複数のIDを含むデータフラグメントを取り込むと、プライベートIDグラフにリンクが作成されます。 IDグラフとIDデータへのアクセスの詳細については、「IDサービスの概要」を参照し [てください](../../identity-service/home.md)。