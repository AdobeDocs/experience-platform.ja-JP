---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したデータセットの作成
topic: datasets
translation-type: tm+mt
source-git-commit: 6d24637dc6cc282f98288b6416e4a3b7cebe42ea

---


# APIを使用したデータセットの作成

このドキュメントでは、Adobe Experience Platform APIを使用してデータセットを作成し、ファイルを使用してデータセットを設定する一般的な手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [バッチインジェスト](../../ingestion/batch-ingestion/overview.md):エクスペリエンスプラットフォームでは、データをバッチファイルとして取り込むことができます。
* [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

* コンテンツタイプ：application/json

## チュートリアル

データセットを作成するには、まずスキーマを定義する必要があります。 スキーマは、データを表すのに役立つ一連のルールです。 スキーマは、データの構造を説明するだけでなく、制約や期待を提供し、システム間でデータが移動される際に、データを適用して検証する際に使用できます。

これらの標準的な定義により、接触チャネルに関係なくデータを一貫して解釈でき、アプリケーション間での翻訳の必要性を排除できます。 スキーマの構成について詳しくは、基本的なスキーマ構成のガイド [を参照してください](../../xdm/schema/composition.md)

## データセットの検索スキーマ

このチュートリアルは、 [スキーマレジストリAPIのチュートリアルが終了した時点で](../../xdm/tutorials/create-schema-api.md) 、チュートリアルの中で作成したロイヤルティメンバースキーマを利用して開始されます。

スキーマレジストリのチュートリアルを完了していない場合は、開始を行い、必要なデータセットを作成した後で、このスキーマのチュートリアルを続行してください。

次の呼び出しを使用して、スキーマレジストリAPIチュートリアルで作成したロイヤルティスキーマを表示できます。

**API形式**

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

応答オブジェクトの形式は、リクエストで送信されるAcceptヘッダーによって異なります。 この応答の個々のプロパティは、領域を最小限に抑えられました。

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

忠誠度メンバースキーマを配置し、そのスキーマを参照するデータセットを作成できます。

**API形式**

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
    },
    "fileDescription": {
        "persisted": true,
        "containerFormat": "parquet",
        "format": "parquet"
    }
}'
```

>[!NOTE] このチュートリアルでは、 [parquet](https://parquet.apache.org/documentation/latest/) （パーケット）ファイル形式を使用してその例を示します。 JSONファイル形式の使用例については、バッチインジェスト開発ガイドを [参照してください](../../ingestion/batch-ingestion/api-overview.md)

**応答**

成功した応答は、HTTP Status 201(Created)と、新しく作成されたデータセットのIDを形式で含む配列で構成される応答オブジェクトを返しま `"@/datasets/{DATASET_ID}"`す。 データセットIDは、API呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```

## バッチの作成

データセットにデータを追加する前に、データセットにリンクするバッチを作成する必要があります。 その後、バッチがアップロードに使用されます。

**API形式**

```HTTP
POST /batches
```

**リクエスト**

リクエスト本文には、前の手順で生成された値を示す「datasetId」フ `{DATASET_ID}` ィールドが含まれています。

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

成功した応答は、HTTP Status 201(Created)と、新しく作成されたバッチの詳細（読み取り専用の、システムで生成された文字列を含む） `id`を含む応答オブジェクトを返します。

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

アップロード用の新しいバッチが正常に作成されたら、特定のデータセットにファイルをアップロードできるようになりました。 データセットを定義する際に、ファイル形式をパーケットとして指定したことを忘れないでください。 したがって、アップロードするファイルはその形式である必要があります。

>[!NOTE] サポートされるデータアップロードファイルの最大サイズは512 MBです。 データファイルのサイズがこれより大きい場合は、512 MB以下のチャンクに分割し、一度に1つずつアップロードする必要があります。 同じバッチIDを使用して、各ファイルに対してこの手順を繰り返すことで、各ファイルを同じバッチにアップロードできます。 ファイルをバッチの一部としてアップロードできる場合、数に制限はありません。

**API形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | アップ `id` ロードするバッチの名前。 |
| `{DATASET_ID}` | バッ `id` チを保持するデータセットの名前。 |
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

正常にアップロードされたファイルは、空の応答本文とHTTPステータス200(OK)を返します。

## シグナルバッチ完了

すべてのデータファイルをバッチにアップロードした後、バッチに完了を知らせることができます。 シグナリングの完了により、サービスは、アップロードされたフ `DataSetFile` ァイルのカタログエントリを作成し、以前に生成されたバッチに関連付けます。 カタログ・バッチは成功とマークされ、ダウンストリーム・フローがトリガーされて、使用可能なデータに対して使用できるようになります。

**API形式**

```HTTP
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | 完了と `id` してマークするバッチのこと。 |

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/5d01230fc78a4e4f8c0c6b387b4b8d1c?action=COMPLETE" \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMG_ORG}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'
```

**応答**

正常に完了したバッチは、空の応答本文とHTTPステータス200(OK)を返します。

## 取り込みの監視

データのサイズに応じて、バッチの取り込みには様々な時間がかかります。 バッチのIDを含むリクエストパラメーターをリクエストに追 `batch` 加することで、バッチのステータスを監視で `GET /batches` きます。 APIは、データセットをポーリングし、バッチの取り込みから、応答内のが完了(「成功」ま `status` たは「失敗」)を示すまでの状態を調べます。

**API形式**

```HTTP
GET /batches?batch={BATCH_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | 監視 `id` するバッチの名前。 |

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

正の応答は、次の値を含む属性を持 `status` つオブジェクトを返しま `success`す。

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

負の応答は、属性にの値を持つオブジェクトを返し、次の関 `"failed"` 連するエ `"status"` ラーメッセージが含まれます。

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

>[!NOTE] 推奨されるポーリング間隔は2分です。

## データセットからのデータの読み取り

バッチIDを使用すると、データアクセスAPIを使用して、バッチにアップロードされたすべてのファイルを読み取り、検証できます。 応答は、ファイルIDのリストを含む配列を返し、それぞれがバッチ内のファイルを参照します。

また、Data Access APIを使用して、名前、サイズ（バイト）、ファイルまたはフォルダーをダウンロードするためのリンクを返すこともできます。

データアクセスAPIを使用する詳細な手順は、『データアクセス開発ガイド』 [を参照してください](../../data-access/api.md)。

## データセットスキーマ

フィールドを追加し、作成したデータセットに追加のデータを取り込むことができます。 これを行うには、まず、新しいデータを定義する追加のスキーマを追加して、データを更新する必要があります。 これは、PATCH操作やPUT操作を使用して、既存の操作を更新することができます。スキーマ

スキーマの更新について詳しくは、 [スキーマレジストリAPI開発者ガイドを参照してください](../../xdm/api/getting-started.md)。

スキーマを更新したら、このチュートリアルの手順に従って、変更後のスキーマに合う新しいデータを取り込みます。

スキーマの進化は純粋に加算的なものであり、スキーマがレジストリに保存され、データの取り込みに使用された後は、変更を中断することはできません。 Adobe Experience Platformで使用するスキーマを構成するためのベストプラクティスについて詳しくは、スキーマ構成の基本に関するガ [イドを参照してください](../../xdm/schema/composition.md)。