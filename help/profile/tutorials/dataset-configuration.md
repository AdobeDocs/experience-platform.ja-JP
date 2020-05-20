---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: APIを使用したプロファイルおよびIDサービスのデータセットの設定
topic: tutorial
translation-type: tm+mt
source-git-commit: 409d98818888f2758258441ea2d993ced48caf9a
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 1%

---


# APIを使用したプロファイルおよびIDサービスのデータセットの設定

このチュートリアルでは、リアルタイム顧客プロファイルおよびIDサービスでデータセットを有効にするプロセスを、次の手順に分けて説明します。

1. 次の2つの方法のいずれかを使用して、リアルタイム顧客プロファイルでのデータセットの使用を有効にします。
   - [新しいデータセットの作成](#create-a-dataset-enabled-for-profile-and-identity)
   - [既存のデータセットの設定](#configure-an-existing-dataset)
1. [データセットにデータを取り込む](#ingest-data-into-the-dataset)
1. [リアルタイム顧客プロファイルによるデータ取り込みの確認](#confirm-data-ingest-by-real-time-customer-profile)
1. [IDサービスによるデータ取り込みの確認](#confirm-data-ingest-by-identity-service)

## はじめに

このチュートリアルでは、プロファイル対応データセットの管理に関連する様々なAdobe Experience Platformサービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、関連するプラットフォームサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [IDサービス](../../identity-service/home.md): プラットフォームに取り込まれる個別のデータソースからIDをブリッジすることで、リアルタイムの顧客プロファイルを有効にします。
- [カタログサービス](../../catalog/home.md): リアルタイム顧客プロファイルおよびIDサービス用のデータセットを作成し、設定できるRESTful API。
- [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。

以下の節では、プラットフォームAPIを正しく呼び出すために知る必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。 プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

- x-sandbox-name: `{SANDBOX_NAME}`

## プロファイルとIDに対応したデータセットの作成 {#create-a-dataset-enabled-for-profile-and-identity}

リアルタイム顧客プロファイルおよびIDサービスのデータセットは、作成後すぐに、またはデータセットの作成後いつでも有効にできます。 作成済みのデータセットを有効にする場合は、このドキュメントの後半に示す既存のデータセット [を](#configure-an-existing-dataset) 設定する手順に従います。 新しいデータセットを作成するには、既存のXDMスキーマのIDが、リアルタイム顧客プロファイルに対して有効になっている必要があります。 プロファイルが有効なスキーマを参照または作成する方法について詳しくは、スキーマレジストリAPIを使用したスキーマの [作成に関するチュートリアルを参照してください](../../xdm/tutorials/create-schema-api.md)。 次のCatalog APIの呼び出しは、プロファイルおよびIDサービスのデータセットを有効にします。

**API形式**

```http
POST /dataSets
```

**リクエスト**

リクエスト本文にデータセット `unifiedProfile``unifiedIdentity``tags` を含めると、そのデータセットのプロファイルがIDサービスに対して即座に有効になります。 これらのタグの値は、文字列を含む配列である必要があり `"enabled:true"`ます。

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
| `schemaRef.id` | プロファイルセットの基になるスキーマ対応データセットのID。 |
| `{TENANT_ID}` | IMS組織に属するスキーマを含むリソースレジストリ内の名前空間。 詳細については、『スキーマレジストリ開発者ガイド』の [TENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) セクションを参照してください。 |

**応答**

正常に完了した場合は、新しく作成されたデータセットのIDがの形式で配列に表示され `"@/dataSets/{DATASET_ID}"`ます。 データセットの作成と有効化が正常に完了したら、データの [アップロード手順に進みます](#upload-data-to-the-dataset)。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

以下の手順では、Real-time Customer Customer Identity Service用に以前に作成したデータセットを有効にする方法を説明します。 プロファイル対応のデータセットを既に作成済みの場合は、データ [取り込みの手順に進んでください](#ingest-data-into-the-dataset)。

### データセットが有効かどうかを確認します。 {#check-if-the-dataset-is-enabled}

カタログAPIを使用して、既存のデータセットを調べ、それがリアルタイム顧客プロファイルおよびIDサービスでの使用に有効になっているかどうかを確認できます。 次の呼び出しは、データセットの詳細をIDで取得します。

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

プロパティの下で、とが両方とも値を持つこ `tags` と `unifiedProfile` を確認でき `unifiedIdentity``enabled:true`ます。 したがって、このデータセットに対しては、それぞれリアルタイム顧客プロファイルとIDサービスが有効になります。

### データセットの有効化 {#enable-the-dataset}

既存のデータセットがプロファイルまたはIDサービスに対して有効になっていない場合は、データセットIDを使用してPATCHリクエストを行うことで有効にできます。

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

リクエスト本文には、2つのサブプロパティを含む `tags` プロパティが含まれています。 `"unifiedProfile"` と `"unifiedIdentity"`。 これらのサブプロパティの値は、文字列を含む配列で `"enabled:true"`す。

**応答** PATCHリクエストが成功すると、HTTP Status 200(OK)と、更新されたデータセットのIDを含む配列が返されます。 このIDは、PATCH要求で送信されたIDと一致する必要があります。 タグ `"unifiedProfile"` と `"unifiedIdentity"` タグが追加され、プロファイルおよびIDサービスでデータセットの使用が有効になりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## データセットにデータを取り込む {#ingest-data-into-the-dataset}

リアルタイム顧客プロファイルとIDサービスは共に、XDMデータをデータセットに取り込む際に消費します。 データセットにデータをアップロードする手順については、APIを使用したデータセットの [作成に関するチュートリアルを参照してください](../../catalog/datasets/create.md)。 プロファイル対応データセットに送信するデータを計画する際には、次のベストプラクティスを考慮してください。

- オーディエンスセグメント条件として使用するデータを含めます。
- IDグラフを最大限に活用するために、プロファイルデータから確認できる限り多くのIDを含めます。 これにより、IDサービスはデータセット間でIDをより効率的に結合できます。

## リアルタイム顧客プロファイルによるデータ取り込みの確認 {#confirm-data-ingest-by-real-time-customer-profile}

初めて新しいデータセットにデータをアップロードする場合、または新しいETLやデータソースに関連するプロセスの一部として、データが期待どおりにアップロードされたかどうかを注意深く確認することをお勧めします。 リアルタイム顧客プロファイルアクセスAPIを使用すると、バッチデータがデータセットに読み込まれるのと同時に取得できます。 期待するエンティティをいずれも取得できない場合は、リアルタイム顧客プロファイルでデータセットが有効になっていない可能性があります。 データセットが有効になっていることを確認したら、ソースデータの形式と識別子が期待どおりに動作することを確認します。 Real-time Customer Customer Apiを使用してプロファイルデータにアクセスする方法について詳しくは、「プロファイルアクセスAPI」とも呼ばれるEntitiesの [サブガイドに従ってください](../api/entities.md)。

## IDサービスによるデータ取り込みの確認 {#confirm-data-ingest-by-identity-service}

複数のIDを含むデータフラグメントを取り込むと、プライベートIDグラフにリンクが作成されます。 IDグラフとIDデータへのアクセスの詳細については、「 [IDサービスの概要](../../identity-service/home.md)」を参照してください。