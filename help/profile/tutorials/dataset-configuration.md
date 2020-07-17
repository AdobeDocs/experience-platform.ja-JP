---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: APIを使用したプロファイルおよびIDサービスのデータセットの設定
topic: tutorial
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 1%

---


# APIのデータセットの設定 [!DNL Profile] と [!DNL Identity Service] 使用

このチュートリアルでは、とで使用するデータセットを有効にするプロセス [!DNL Real-time Customer Profile] を、次の手順 [!DNL Identity Service]に分けて説明します。

1. 次の2つのオプションのいずれかを使用して、でのデータセットの使用 [!DNL Real-time Customer Profile]を有効にします。
   - [新しいデータセットの作成](#create-a-dataset-enabled-for-profile-and-identity)
   - [既存のデータセットの設定](#configure-an-existing-dataset)
1. [データセットにデータを取り込む](#ingest-data-into-the-dataset)
1. [リアルタイム顧客プロファイルによるデータ取り込みの確認](#confirm-data-ingest-by-real-time-customer-profile)
1. [IDサービスによるデータ取り込みの確認](#confirm-data-ingest-by-identity-service)

## はじめに

このチュートリアルでは、 [!DNL Profile]有効なデータセットの管理に関連する様々なAdobe Experience Platformサービスについて、十分な理解を得る必要があります。 このチュートリアルを開始する前に、次の関連 [!DNL Platform] サービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [!DNL Identity Service](../../identity-service/home.md): 取り込ま [!DNL Real-time Customer Profile] れる異なるデータソースのIDをブリッジ化することで有効に [!DNL Platform]します。
- [!DNL Catalog Service](../../catalog/home.md): および用のデータセットの作成と設定を可能にするRESTful API [!DNL Real-time Customer Profile] で [!DNL Identity Service]す。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

以下の節では、PlatformAPIを正しく呼び出すために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。 のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

- x-sandbox-name: `{SANDBOX_NAME}`

## 有効にするデータセットの作成( [!DNL Profile] および [!DNL Identity] {#create-a-dataset-enabled-for-profile-and-identity}

データセットの作成 [!DNL Real-time Customer Profile] 時および作成 [!DNL Identity Service] 後の任意の時点で、データセットを有効にすることができます。 作成済みのデータセットを有効にする場合は、このドキュメントの後半に示す既存のデータセット [を](#configure-an-existing-dataset) 設定する手順に従います。 新しいデータセットを作成するには、既存のXDMスキーマのIDが、リアルタイム顧客プロファイルに対して有効になっている必要があります。 プロファイルが有効なスキーマを参照または作成する方法について詳しくは、スキーマレジストリAPIを使用したスキーマの [作成に関するチュートリアルを参照してください](../../xdm/tutorials/create-schema-api.md)。 次の [!DNL Catalog] APIの呼び出しは、およびのデータセットを有効に [!DNL Profile] し [!DNL Identity Service]ます。

**API形式**

```http
POST /dataSets
```

**リクエスト**

リクエストの本文にとを含め `unifiedProfile` ると、リクエストの本文の下にデータセットが直ちに有効になり `unifiedIdentity` 、 `tags`[!DNL Profile][!DNL Identity Service]とに対してデータセットがすぐに有効になります。 これらのタグの値は、文字列を含む配列である必要があり `"enabled:true"`ます。

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
| `schemaRef.id` | データセットの基になる [!DNL Profile]有効なスキーマのID。 |
| `{TENANT_ID}` | IMS組織に属する名前空間 [!DNL Schema Registry] が含まれる、内のリソース。 詳細については、『 [開発者ガイド』の「](../../xdm/api/getting-started.md#know-your-tenant-id) TENANT_ID [!DNL Schema Registry] 」の節を参照してください。 |

**応答**

正常に完了した場合は、新しく作成されたデータセットのIDがの形式で配列に表示され `"@/dataSets/{DATASET_ID}"`ます。 データセットの作成と有効化が正常に完了したら、データの [アップロード手順に進みます](#upload-data-to-the-dataset)。

```json
[
    "@/dataSets/5b020a27e7040801dedbf46e"
] 
```

## 既存のデータセットの設定 {#configure-an-existing-dataset}

以下の手順では、 [!DNL Real-time Customer Profile] およびに対して以前に作成したデータセットを有効にする方法について説明 [!DNL Identity Service]します。 プロファイル対応のデータセットを既に作成済みの場合は、データ [取り込みの手順に進んでください](#ingest-data-into-the-dataset)。

### データセットが有効かどうかを確認します。 {#check-if-the-dataset-is-enabled}

APIを使用して、既存のデータセットを調べ、そのデータセットのとでの使用が有効になっているかどうかを確認でき [!DNL Catalog] ま [!DNL Real-time Customer Profile][!DNL Identity Service]す。 次の呼び出しは、データセットの詳細をIDで取得します。

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

プロパティの下で、とが両方とも値を持つこ `tags` と `unifiedProfile` を確認でき `unifiedIdentity``enabled:true`ます。 したがって、 [!DNL Real-time Customer Profile][!DNL Identity Service] とは、それぞれこのデータセットに対して有効になります。

### データセットの有効化 {#enable-the-dataset}

またはに対して既存のデータセットが有効になっていない場合 [!DNL Profile][!DNL Identity Service]は、データセットIDを使用してPATCHリクエストを行うことで有効にできます。

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

とは、XDMデータ [!DNL Real-time Customer Profile] がデータセットに取り込まれる際に、その両方を [!DNL Identity Service] 利用します。 データセットにデータをアップロードする手順については、APIを使用したデータセットの [作成に関するチュートリアルを参照してください](../../catalog/datasets/create.md)。 有効なデータセットに送信するデータを計画する際には、次のベストプラクティスを考慮し [!DNL Profile]てください。

- オーディエンスセグメント条件として使用するデータを含めます。
- IDグラフを最大限に活用するために、プロファイルデータから確認できる限り多くのIDを含めます。 これにより、データセット間でIDをより効率的 [!DNL Identity Service] に結合できます。

## データ取り込みの確認 [!DNL Real-time Customer Profile] {#confirm-data-ingest-by-real-time-customer-profile}

初めて新しいデータセットにデータをアップロードする場合、または新しいETLやデータソースに関連するプロセスの一部として、データが期待どおりにアップロードされたかどうかを注意深く確認することをお勧めします。 Access APIを使用すると、バッチデータがデータセットに読み込まれるたびに取得できます。 [!DNL Real-time Customer Profile] 期待するエンティティをいずれも取得できない場合は、データセットが有効になっていない可能性があり [!DNL Real-time Customer Profile]ます。 データセットが有効になっていることを確認したら、ソースデータの形式と識別子が期待どおりに動作することを確認します。 APIを使用して [!DNL Real-time Customer Profile] データにアクセスする方法について詳しくは、「 [!DNL Profile] API」とも呼ばれる [エンティティのエンドポイントガイド](../api/entities.md)[!DNL Profile Access] に従ってください。

## IDサービスによるデータ取り込みの確認 {#confirm-data-ingest-by-identity-service}

複数のIDを含むデータフラグメントを取り込むと、プライベートIDグラフにリンクが作成されます。 IDグラフとIDデータへのアクセスの詳細については、「 [IDサービスの概要](../../identity-service/home.md)」を参照してください。