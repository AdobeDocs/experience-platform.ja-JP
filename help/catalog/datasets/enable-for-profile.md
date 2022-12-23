---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API;データセットの有効化
title: API を使用したプロファイルおよび ID サービスのデータセットを有効にする
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform API を使用して、リアルタイム顧客プロファイルおよび ID サービスでデータセットを有効にする方法について説明します。
exl-id: a115e126-6775-466d-ad7e-ee36b0b8b49c
source-git-commit: 132407af947b97a1925799a1fb5e12caa2b0410c
workflow-type: ht
source-wordcount: '1073'
ht-degree: 100%

---

# API を使用して [!DNL Profile] と [!DNL Identity Service] のデータセットを有効にする

このチュートリアルでは、[!DNL Real-time Customer Profile] および [!DNL Identity Service] で使用するデータセットを有効にするプロセスを、次の手順に分けて説明します。

1. 次の 2 つのオプションのいずれかを使用して、[!DNL Real-time Customer Profile] で使用するデータセットを有効にします。
   - [新しいデータセットの作成](#create-a-dataset-enabled-for-profile-and-identity)
   - [既存のデータセットの設定](#configure-an-existing-dataset)
1. [データセットへのデータの取り込み](#ingest-data-into-the-dataset)
1. [リアルタイム顧客プロファイルによるデータ取り込みの確認](#confirm-data-ingest-by-real-time-customer-profile)
1. [ID サービスによるデータ取り込みの確認](#confirm-data-ingest-by-identity-service)

## はじめに

このチュートリアルでは、プロファイル対応データセットに関連する様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、これらの関連 [!DNL Platform] サービスに関するドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Identity Service]](../../identity-service/home.md)：[!DNL Real-time Customer Profile] に取り込まれる様々なデータソースの ID を結合することで、[!DNL Platform] を有効にします。
- [[!DNL Catalog Service]](../../catalog/home.md)：データセットを作成し、[!DNL Real-time Customer Profile] と [!DNL Identity Service] に設定できる RESTful API。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

以下の節では、Platform API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、追加の `Content-Type` ヘッダーが必要です。必要に応じて、このヘッダーの正しい値がサンプルリクエストに表示されます。

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定する `x-sandbox-name` ヘッダーが必要です。[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## プロファイルおよび ID 対応データセットの作成 {#create-a-dataset-enabled-for-profile-and-identity}

リアルタイム顧客プロファイルおよび ID サービスのデータセットは、作成時、またはデータセットの作成後任意の時点で有効にできます。作成済みのデータセットを有効にする場合は、このドキュメントの後半の手順に従って、[既存のデータセットを設定](#configure-an-existing-dataset)します。

>[!NOTE]
>
>プロファイルを有効にしたデータセットを新規作成するには、プロファイルを有効にした既存 XDM スキーマの ID を知っておく必要があります。プロファイルが有効なスキーマを参照または作成する方法について詳しくは、[スキーマレジストリ API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを参照してください。

プロファイルに対して有効なデータセットを作成するには、`/dataSets` エンドポイントへの POST リクエストを使用できます。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

リクエスト本文の `tags` に `unifiedProfile` と `unifiedIdentity` を含めることで、データセットは、[!DNL Profile] と [!DNL Identity Service] に対して直ちに有効になります。これらのタグの値は、`"enabled:true"` 文字列を含む配列である必要があります。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "fields":[],
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/31670881463308a46f7d2cb09762715",
        "contentType": "application/vnd.adobe.xed-full-notext+json; version=1"
    },
    "tags": {
       "unifiedProfile": ["enabled:true"],
       "unifiedIdentity": ["enabled:true"]
    }
  }'
```

| プロパティ | 説明 |
|---|---|
| `schemaRef.id` | データセットの基となる [!DNL Profile] 対応スキーマの ID。 |
| `{TENANT_ID}` | [!DNL Schema Registry] 内の名前空間で、IMS 組織に属するリソースが含まれます。詳しくは、[!DNL Schema Registry] 開発者ガイドの [TENANT_ID](../../xdm/api/getting-started.md#know-your-tenant-id) の節を参照してください。 |

**応答** 

正常に完了すると、新しく作成されたデータセットの ID を含む配列が、`"@/dataSets/{DATASET_ID}"`の形式で表示されます。データセットの作成と有効化が完了したら、[データのアップロード](#upload-data-to-the-dataset)手順に進んでください。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

以下の手順では、以前に作成した [!DNL Real-time Customer Profile] と [!DNL Identity Service] のデータセットを有効にする方法を説明します。 既にプロファイル対応データセットを作成している場合は、[データの取り込み](#ingest-data-into-the-dataset)手順に進んでください。

### データセットが有効かどうかを確認します。 {#check-if-the-dataset-is-enabled}

[!DNL Catalog] API を使用すると、既存のデータセットを調べて、それが [!DNL Real-time Customer Profile] および [!DNL Identity Service] での使用に対して有効になっているかどうかを判断できます。次の呼び出しは、データセットの詳細を ID によって取得します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{
    "5b020a27e7040801dedbf46e": {
        "name": "Commission Program Events DataSet",
        "imsOrg": "{ORG_ID}",
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
        "transforms": "@/dataSets/5b020a27e7040801dedbf46e/views/5b020a27e7040801dedbf46f/transforms",
        "files": "@/dataSets/5b020a27e7040801dedbf46e/views/5b020a27e7040801dedbf46f/files",
        "schema": "@/xdms/context/experienceevent",
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

プロパティ `tags` の下で、`unifiedProfile` と `unifiedIdentity` が値 `enabled:true` と共に存在することを確認できます。したがって、このデータセットでは [!DNL Real-time Customer Profile] および [!DNL Identity Service] がそれぞれ有効になっています。

### データセットの有効化 {#enable-the-dataset}

既存のデータセットが [!DNL Profile] または [!DNL Identity Service] に対して有効になっていない場合は、データセット ID を使用して PATCH リクエストを行うことで有効化できます。

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
  -H 'Content-Type:application/json-patch+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/tags/unifiedProfile", "value": ["enabled:true"] },
        { "op": "add", "path": "/tags/unifiedIdentity", "value": ["enabled:true"] } 
      ]'
```

リクエスト本文には、`path` から `unifiedProfile` と `unifiedIdentity` の 2 種類のタグが含まれます。それぞれの `value` は、文字列 `enabled:true` を含む配列です。

**応答** PATCH リクエストが成功すると、HTTP ステータス 200（OK）と、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致する必要があります。`unifiedProfile` タグと `unifiedIdentity` タグが追加され、データセットがプロファイルおよび ID サービスで使用できるようになりました。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
]
```

## データセットへのデータの取り込み {#ingest-data-into-the-dataset}

[!DNL Real-time Customer Profile] および [!DNL Identity Service] は両方ともデータセットに取り込まれる際に XDM データを使用します。 データセットにデータをアップロードする手順については、[API を使用したデータセットの作成](../../catalog/datasets/create.md)に関するチュートリアルを参照してください。[!DNL Profile] 対応データセットに送信するデータを計画する際には、次のベストプラクティスを考慮してください。

- セグメント化条件として使用するデータを含めます。
- ID グラフを最大化するために、プロファイルデータから確認できる識別子をできるだけ多く含めます。これにより、[!DNL Identity Service] は、データセット間で ID をより効果的に結び付けることができます。

## [!DNL Real-time Customer Profile] によるデータの取り込みを確認します。 {#confirm-data-ingest-by-real-time-customer-profile}

初めて新しいデータセットにデータをアップロードする際、または新しい ETL やデータソースが関与するプロセスの一部として、データが期待どおりにアップロードされたかどうかを慎重に確認することをお勧めします。[!DNL Real-time Customer Profile] Access API を使用すると、データセットに読み込まれるバッチデータを取得できます。目的のエンティティを取得できない場合は、[!DNL Real-time Customer Profile] でデータセットが有効になっていない可能性があります。データセットが有効になっていることを確認した後、ソースデータの形式と識別子が期待通りに動作することを確認します。[!DNL Real-time Customer Profile] API を使用して [!DNL Profile] データにアクセスする方法の詳細な手順については、[エンティティエンドポイントガイド](../../profile/api/entities.md)（「[!DNL Profile Access]」API とも呼ばれる）を参照してください。

## ID サービスによるデータ取り込みの確認 {#confirm-data-ingest-by-identity-service}

複数の ID を含むデータフラグメントを取り込むと、プライベート ID グラフにリンクが作成されます。ID グラフと ID データへのアクセスの詳細については、[ID サービスの概要](../../identity-service/home.md)を参照してください。
