---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したデータセットの作成
topic: datasets
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '1234'
ht-degree: 1%

---


# APIを使用したデータセットの作成

このドキュメントでは、Adobe Experience PlatformAPIを使用してデータセットを作成し、ファイルを使用してデータセットを設定する一般的な手順について説明します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [バッチインジェスト](../../ingestion/batch-ingestion/overview.md): [!DNL Experience Platform] データをバッチファイルとして取り込むことができます。
* [!DNL Experience Data Model (XDM) System](../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
* [!DNL Sandboxes](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、APIを正しく呼び出すために知る必要がある追加情報について説明し [!DNL Platform] ます。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## チュートリアル

データセットを作成するには、まずスキーマを定義する必要があります。 スキーマは、データを表すのに役立つ一連のルールです。 スキーマは、データの構造を説明するだけでなく、制約や期待を設け、システム間を移動する際にデータを適用して検証できるようにします。

これらの標準の定義により、接触チャネルに関係なく一貫した形でデータを解釈でき、複数のアプリケーション間での翻訳を必要としなくなります。 スキーマの構成について詳しくは、スキーマ構成の [基本に関するガイドを参照してください](../../xdm/schema/composition.md)

## データセットスキーマの検索

このチュートリアルは、 [スキーマレジストリAPIのチュートリアルが終了した時点で開始され](../../xdm/tutorials/create-schema-api.md) 、そのチュートリアルで作成したロイヤルティメンバーのスキーマを利用します。

この [!DNL Schema Registry] チュートリアルを完了していない場合は、開始を進め、必要なスキーマを構成した後で、このデータセットのチュートリアルを続けてください。

次の呼び出しを使用して、 [!DNL Schema Registry] APIチュートリアルで作成した忠誠度メンバースキーマを表示できます。

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

応答オブジェクトの形式は、リクエストで送信されるAcceptヘッダーによって異なります。 この応答内の個々のプロパティが最小化され、領域が確保されました。

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

Loyality Membersスキーマを配置した状態で、スキーマを参照するデータセットを作成できるようになりました。

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

>[!NOTE]
>
>このチュートリアルでは、 [parket](https://parquet.apache.org/documentation/latest/) file形式を使用してそのすべての例を示します。 JSONファイル形式の使用例については、『 [batch ingestion developer guide』を参照してください](../../ingestion/batch-ingestion/api-overview.md)

**応答**

正常に完了すると、HTTPステータス201（作成済み）と、新たに作成されたデータセットのIDを形式で含む配列で構成される応答オブジェクトが返され `"@/datasets/{DATASET_ID}"`ます。 データセットIDは、API呼び出しでデータセットを参照するために使用される、読み取り専用の、システム生成の文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```

## バッチの作成

データセットにデータを追加する前に、データセットにリンクするバッチを作成する必要があります。 このバッチがアップロードに使用されます。

**API形式**

```HTTP
POST /batches
```

**リクエスト**

リクエスト本文には「datasetId」フィールドが含まれ、その値は前の手順で `{DATASET_ID}` 生成された値です。

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

正常な応答は、HTTPステータス201（作成済み）と、新しく作成されたバッチの詳細（読み取り専用の、システムで生成された文字列を含む） `id`を含む応答オブジェクトを返します。

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

## ファイルをバッチにアップロード

アップロード用の新しいバッチが正常に作成された後に、特定のデータセットにファイルをアップロードできるようになりました。 データセットを定義する際、ファイル形式をパーケットとして指定したことを忘れないでください。 したがって、アップロードするファイルはその形式にする必要があります。

>[!NOTE]
>
>サポートされるデータアップロードファイルのサイズは512 MBです。 データファイルのサイズがこれより大きい場合は、512 MB以下のチャンクに分割し、一度に1つずつアップロードする必要があります。 同じバッチIDを使用して、各ファイルに対してこの手順を繰り返すことで、同じバッチ内の各ファイルをアップロードできます。 ファイルをバッチの一部としてアップロードできる場合、数に制限はありません。

**API形式**

```http
PUT /batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | アップロード `id` するバッチの名前。 |
| `{DATASET_ID}` | バッチ `id` が保持されるデータセットの名前。 |
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

すべてのデータファイルをバッチにアップロードした後、バッチに完了を知らせることができます。 シグナリングの完了により、サービスはアップロードされたファイルの [!DNL Catalog]`DataSetFile` エントリを作成し、以前に生成されたバッチに関連付けます。 バッチは成功とマークされ、ダウンストリームフローがトリガーされ、使用可能なデータに対して使用できるようになります。 [!DNL Catalog]

**API形式**

```HTTP
POST /batches/{BATCH_ID}?action=COMPLETE
```

| パラメーター | 説明 |
| --- | --- |
| `{BATCH_ID}` | 完了 `id` としてマークするバッチの名前。 |

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/5d01230fc78a4e4f8c0c6b387b4b8d1c?action=COMPLETE" \
  -H 'x-api-key : {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMG_ORG}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'
```

**応答**

正常に完了したバッチは、空の応答本文とHTTPステータス200(OK)を返します。

## モニターの取り込み

データのサイズに応じて、バッチの取り込みには様々な時間がかかります。 バッチのIDを含むリクエストパラメーターをリクエストに追加することで、バッチのステータスを監視でき `batch``GET /batches` ます。 APIはデータセットをポーリングし、取り込みから、応答内のが完了（「成功」または「失敗」） `status` を示すまで、バッチのステータスを調べます。

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

正の応答は、次の値を含む `status` 属性を持つオブジェクトを返し `success`ます。

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

負の応答は、属性にの値を持つオブジェクトを返し `"failed"``"status"` 、関連するエラーメッセージが含まれます。

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
>推奨されるポーリング間隔は2分です。

## データセットからのデータの読み取り

バッチIDを使用すると、Data Access APIを使用して、バッチにアップロードされたすべてのファイルを読み取り、検証できます。 この応答は、ファイルIDのリストを含む配列を返し、各要素はバッチ内のファイルを参照します。

また、Data Access APIを使用して、名前、サイズ（バイト単位）、ファイルまたはフォルダーをダウンロードするためのリンクを返すこともできます。

Data Access APIを使用する詳細な手順については、 [データアクセス開発ガイドを参照してください](../../data-access/home.md)。

## データセットスキーマの更新

フィールドを追加し、作成したデータセットに追加のデータを取り込むことができます。 これを行うには、最初に、新しいデータを定義するプロパティを追加してスキーマを更新する必要があります。 これは、PATCH操作やPUT操作を使用して既存のスキーマを更新することで可能です。

スキーマの更新について詳しくは、『 [スキーマレジストリAPI開発者ガイド](../../xdm/api/getting-started.md)』を参照してください。

スキーマを更新したら、このチュートリアルの手順に従って、変更後のスキーマに合致する新しいデータを取り込みます。

スキーマの進化は純粋に付加的なものであり、レジストリに保存されてデータ取り込みに使用された後は、スキーマに中断点の変更を加えることはできません。 Adobe Experience Platformで使用するスキーマを構成するためのベストプラクティスについて詳しくは、スキーマ構成の [基本に関するガイドを参照してください](../../xdm/schema/composition.md)。