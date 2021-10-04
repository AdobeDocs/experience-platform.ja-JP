---
keywords: Experience Platform；ホーム；よく読まれるトピック；データセット；データセット；データセットの作成；データセットの作成
solution: Experience Platform
title: API を使用したデータセットの作成
topic-legacy: datasets
description: このドキュメントでは、Adobe Experience Platform API を使用してデータセットを作成し、ファイルを使用してデータセットを設定する一般的な手順を説明します。
exl-id: 3a5f48cf-ad05-4b9e-be1d-ff213a26a477
source-git-commit: e4bf5bb77ac4186b24580329699d74d653310d93
workflow-type: tm+mt
source-wordcount: '1305'
ht-degree: 85%

---

# API を使用したデータセットの作成

このドキュメントでは、Adobe Experience Platform API を使用してデータセットを作成し、ファイルを使用してデータセットを設定する一般的な手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [バッチ取得](../../ingestion/batch-ingestion/overview.md): [!DNL Experience Platform] では、データをバッチファイルとして取り込むことができます。
* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、単一のインスタンスを別々の仮想環境に分 [!DNL Platform] 割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Platform] API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type: application/json

## チュートリアル

データセットを作成するには、まずスキーマを定義する必要があります。スキーマは、データを表すのに役立つ一連のルールです。スキーマは、データの構造を説明するだけでなく、システム間を移動するデータを検証するために適用および使用できる制約と期待を提供します。

これらの標準的な定義により、タッチチャネルに関係なくデータを一貫して解釈でき、アプリケーション間での翻訳の必要性を排除できます。スキーマの構成について詳しくは、[基本的なスキーマ構成のガイド](../../xdm/schema/composition.md)を参照してください。

## データセットスキーマの検索

このチュートリアルは、[スキーマレジストリ API チュートリアル](../../xdm/tutorials/create-schema-api.md)が終わったところから始まり、チュートリアルの中で作成したロイヤルティメンバースキーマを利用します。

[!DNL Schema Registry] チュートリアルを完了していない場合は、まず始めて、必要なスキーマを構成した後で、このデータセットチュートリアルを続行してください。

次の呼び出しを使用して、 [!DNL Schema Registry] API チュートリアルで作成した「ロイヤルティメンバー」スキーマを表示できます。

**API 形式**

```HTTP
GET /tenant/schemas/{schema meta:altId or URL encoded $id URI}
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答オブジェクトの形式は、リクエストで送信される Accept ヘッダーによって異なります。この応答の個々のプロパティは、スペースを節約するために最小化にされました。

```JSON
{
    "type": "object",
    "title": "Loyalty Members",
    "description": "Information for all members of the loyalty program",
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/common/extensible",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/{TENANT_ID}/mixins/bb118e507bb848fd85df68fedea70c62"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:immutableTags": [
        "union"
    ],
    "meta:altId": "_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9",
    "meta:xdmType": "object",
    "properties": {
        "repositoryCreatedBy": {},
        "repositoryLastModifiedBy": {},
        "createdByBatchID": {},
        "modifiedByBatchID": {},
        "_repo": {},
        "identityMap": {},
        "_id": {},
        "timeSeriesEvents": {},
        "person": {},
        "homeAddress": {},
        "personalEmail": {},
        "homePhone": {},
        "mobilePhone": {},
        "faxPhone": {},
        "_{TENANT_ID}": {
            "type": "object",
            "meta:xdmType": "object",
            "properties": {
                "loyalty": {
                    "title": "Loyalty",
                    "description": "Loyalty Info",
                    "type": "object",
                    "meta:xdmType": "object",
                    "meta:referencedFrom": "https://ns.adobe.com/{TENANT_ID}/datatypes/49b594dabe6bec545c8a6d1a0991a4dd",
                    "properties": {
                        "loyaltyId": {
                            "title": "Loyalty Identifier",
                            "type": "string",
                            "description": "Loyalty Identifier.",
                            "meta:xdmType": "string"
                        },
                        "loyaltyLevel": {
                            "title": "Loyalty Level",
                            "type": "string",
                            "meta:xdmType": "string"
                        },
                        "loyaltyPoints": {
                            "title": "Loyalty Points",
                            "type": "integer",
                            "description": "Loyalty points total.",
                            "meta:xdmType": "int"
                        },
                        "memberSince": {
                            "title": "Member Since",
                            "type": "string",
                            "format": "date-time",
                            "description": "Date the member joined the Loyalty Program.",
                            "meta:xdmType": "date-time"
                        }
                    }
                }
            }
        }
    },
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/533ca5da28087c44344810891b0f03d9",
    "version": "1.4",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1551836845496,
        "repo:lastModifiedDate": 1551843052271,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

## データセットの作成

「ロイヤルティメンバー」スキーマが完成したら、そのスキーマを参照するデータセットを作成できます。

**API 形式**

```HTTP
POST /dataSets
```

**リクエスト**

```SHELL
curl -X POST \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?requestDataSource=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name":"LoyaltyMembersDataset",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/719c4e19184402c27595e65b931a142b",
        "contentType": "application/vnd.adobe.xed+json;version=1"
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `schemaRef.id` | データセットの基になる XDM スキーマの URI `$id` 値。 |
| `schemaRef.contentType` | スキーマの形式とバージョンを示します。 詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。 |

>[!NOTE]
>
>このチュートリアルでは、すべての例で [Apache Parquet](https://parquet.apache.org/documentation/latest/) ファイル形式を使用します。 JSON ファイル形式の使用例については、[バッチ取得開発ガイド](../../ingestion/batch-ingestion/api-overview.md)を参照してください。

**応答** 

成功した応答は、HTTP Status 201（作成済み）と、新しく作成されたデータセットの ID を `"@/datasets/{DATASET_ID}"` 形式で含む配列で構成される応答オブジェクトを返します。データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```

## バッチの作成

データセットにデータを追加する前に、データセットにリンクするバッチを作成する必要があります。その後、バッチがアップロードに使用されます。

**API 形式**

```HTTP
POST /batches
```

**リクエスト**

リクエスト本文には、前の手順で生成された `{DATASET_ID}` の値を示す「datasetId」フィールドが含まれています。

```SHELL
curl -X POST 'https://platform.adobe.io/data/foundation/import/batches' \
  -H 'accept: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'content-type: application/json' \
  -d '{
        "datasetId":"5c8c3c555033b814b69f947f"
      }'
```

**応答** 

成功した応答は、HTTP ステータス 201（作成済み）と、新しく作成されたバッチの詳細を含む応答オブジェクトを返します。これには、読み取り専用のシステム生成文字列である `id` が含まれます。

```JSON
{
    "id": "5d01230fc78a4e4f8c0c6b387b4b8d1c",
    "imsOrg": "{IMS_ORG}",
    "updated": 1552694873602,
    "status": "loading",
    "created": 1552694873602,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "5c8c3c555033b814b69f947f"
        }
    ],
    "version": "1.0.0",
    "tags": {
        "acp_producer": [
            "{CREATED_CLIENT}"
        ],
        "acp_stagePath": [
            "{CREATED_CLIENT}/stage/5d01230fc78a4e4f8c0c6b387b4b8d1c"
        ],
        "use_plan_b_batch_status": [
            "false"
        ]
    },
    "createdUser": "{CREATED_BY}",
    "updatedUser": "{CREATED_BY}",
    "externalId": "5d01230fc78a4e4f8c0c6b387b4b8d1c",
    "createdClient": "{CREATED_CLIENT}",
    "inputFormat": {
        "format": "parquet"
    }
}
```

## ファイルのバッチへのアップロード

アップロード用の新しいバッチが正常に作成されたら、特定のデータセットにファイルをアップロードできるようになりました。データセットを定義する際に、ファイル形式を Parquet に指定したことを忘れないでください。 したがって、アップロードするファイルはその形式である必要があります。

>[!NOTE]
>
> サポートされるデータアップロードファイルの最大サイズは 512 MB です。データファイルのサイズがこれより大きい場合は、512 MB 以下のチャンクに分割し、一度に 1 つずつアップロードする必要があります。同じバッチ ID を使用して、各ファイルに対してこの手順を繰り返すことで、各ファイルを同じバッチにアップロードできます。ファイルをバッチの一部としてアップロードできる場合、数に制限はありません。

**API 形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | アップロード先のバッチの `id`。 |
| `{DATASET_ID}` | バッチを保持するデータセットの `id`。 |
| `{FILE_NAME}` | アップロードするファイルの名前。 |

**リクエスト**

```SHELL
curl -X PUT 'https://platform.adobe.io/data/foundation/import/batches/5d01230fc78a4e4f8c0c6b387b4b8d1c/datasets/5c8c3c555033b814b69f947f/files/loyaltyData.parquet' \
  -H 'content-type: application/octet-stream' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMG_ORG}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  --data-binary '@{FILE_PATH_AND_NAME}.parquet'
```

**応答** 

正常にアップロードされたファイルは、空の応答本文と HTTP ステータス 200（OK）を返します。

## シグナルバッチ完了

すべてのデータファイルをバッチにアップロードした後、バッチに完了を知らせることができます。完了を通知すると、サービスはアップロードされたファイルの [!DNL Catalog] `DataSetFile` エントリを作成し、それらを以前に生成されたバッチに関連付けます。 [!DNL Catalog] バッチは成功とマークされ、ダウンストリームフローがトリガーされ、使用可能なデータに対して機能するようになります。

**API 形式**

```HTTP
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | 完了としてマークするバッチの `id`。 |

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/5d01230fc78a4e4f8c0c6b387b4b8d1c?action=COMPLETE" \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMG_ORG}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'
```

**応答** 

正常に完了したバッチは、空の応答本文と HTTP ステータス 200（OK）を返します。

## 取得の監視

データのサイズに応じて、バッチ取得には様々な時間がかかります。バッチの ID を含む `batch` リクエストパラメーターを `GET /batches` リクエストに追加することで、バッチのステータスを監視できます。API は、データセットをポーリングし、バッチの取得から、応答内の `status` が完了（「成功」または「失敗」）を示すまでの状態を調べます。

**API 形式**

```HTTP
GET /batches?batch={BATCH_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | 監視するバッチの `id`。 |

**リクエスト**

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/batches?batch=5d01230fc78a4e4f8c0c6b387b4b8d1c' \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMG_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'
```

**応答** 

正の応答は、`success` の値を含む `status` 属性を持つオブジェクトを返しま す。

```JSON
{
    "5b7129a879323401ef2a6486": {
        "imsOrg": "{IMS_ORG}",
        "created": 1534142888068,
        "createdClient": "{CREATED_CLIENT}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "updated": 1534142955152,
        "replay": {},
        "status": "success",
        "errors": [],
        "version": "1.0.3",
        "availableDates": {},
        "relatedObjects": [
            {
                "type": "batch",
                "id": "29285e08378f4a41827e7e70fb7cb8f0"
            }
        ],
        "metrics": {
            "startTime": 1534142943819,
            "endTime": 1534142951760,
            "recordsRead": 108,
            "recordsWritten": 108
        }
    }
}
```

負の応答は、`"status"` 属性に `"failed"` の値を持つオブジェクトを返し、次の関連するエラーメッセージが含まれます。

```JSON
{
    "5b96ce65badcf701e51f075d": {
        "imsOrg": "{IMS_ORG}",
        "status": "failed",
        "relatedObjects": [
            {
                "type": "batch",
                "id": "29285e08378f4a41827e7e70fb7cb8f0"
            }
        ],
        "replay": {},
        "availableDates": {},
        "metrics": {
            "startTime": 1536610322329,
            "endTime": 1536610438083,
            "recordsRead": 4004,
            "recordsWritten": 4004,
            "failureReason": "Job aborted due to stage failure: Task 0 in stage 1.0 failed 4 times,:"
        },
        "errors": [
            {
                "code": "0070000017",
                "description": "Unknown error occurred."
            },
            {
                "code": "unknown",
                "description": "Job aborted."
            }
        ],
        "created": 1536609893629,
        "createdClient": "{CREATED_CLIENT}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "updated": 1536610442814,
        "version": "1.0.5"
    }
}
```

>[!NOTE]
>
> 推奨されるポーリング間隔は 2 分です。

## データセットからのデータの読み取り

バッチ ID を使用すると、データアクセス API を使用して、バッチにアップロードされたすべてのファイルを読み戻し、確認できます。応答は、ファイル ID のリストを含む配列を返し、それぞれがバッチ内のファイルを参照します。

また、データアクセス API を使用して、名前、サイズ（バイト）、ファイルまたはフォルダーをダウンロードするためのリンクを返すこともできます。

データアクセス API を使用する詳細な手順は、『[データアクセス開発ガイド](../../data-access/home.md)』を参照してください。

## データセットスキーマの更新

フィールドを追加し、作成したデータセットに追加のデータを取得することができます。これをおこなうには、まず、新しいデータを定義する追加のスキーマを追加して、データを更新する必要があります。これは、PATCH 操作や PUT 操作を使用して、既存のスキーマを更新することができます。

スキーマの更新について詳しくは、『[スキーマレジストリ API 開発者ガイド](../../xdm/api/getting-started.md)を参照してください』。

スキーマを更新したら、このチュートリアルの手順に従って、変更後のスキーマに合う新しいデータを取得します。

スキーマの進化は純粋に付加的なものであることを覚えておくことが重要です。つまり、スキーマをレジストリに保存してデータの取得に使用すると、スキーマに重大な変更を加えることはできません。Adobe Experience Platform で使用するスキーマを構成するためのベストプラクティスについて詳しくは、[スキーマ構成の基本に関するガイド](../../xdm/schema/composition.md)を参照してください。
