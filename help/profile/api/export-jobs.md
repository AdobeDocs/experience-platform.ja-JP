---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: プロファイル書き出しジョブ API エンドポイント
type: Documentation
description: リアルタイム顧客プロファイルを使用すると、属性データと行動データの両方を含む複数のソースからデータを統合することで、Adobe Experience Platform内で個々の顧客の単一のビューを作成できます。 その後、さらに処理するために、プロファイルデータをデータセットに書き出すことができます。
role: Developer
exl-id: d51b1d1c-ae17-4945-b045-4001e4942b67
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 53%

---

# プロファイル書き出しジョブエンドポイント

[!DNL Real-Time Customer Profile] を使用すると、属性データと行動データの両方を含む複数のソースからデータを統合することで、個々の顧客の単一のビューを作成できます。 その後、さらに処理するために、プロファイルデータをデータセットに書き出すことができます。 例えば、オーディエンスを作成することで、アクティベーション用に [!DNL Profile] データを書き出したり、レポート用にプロファイル属性を書き出したりできます。

このドキュメントでは、[Profile API](https://www.adobe.com/go/profile-apis-en) を使用してエクスポートジョブを作成および管理する手順を説明します。

>[!NOTE]
>
>このガイドでは、[!DNL Profile API] での書き出しジョブの使用について説明します。 Adobe Experience Platform Segmentation Service の書き出しジョブの管理方法については、[Segmentation API での書き出しジョブ ](../../profile/api/export-jobs.md) に関するガイドを参照してください。

エクスポートジョブの作成に加えて、`/entities` エンドポイント（「[!DNL Profile Access]」とも呼ばれる）を使用して [!DNL Profile] データにアクセスすることもできます。 詳しくは、[ エンティティエンドポイントガイド ](./entities.md) を参照してください。 UI を使用してデータにアクセス [!DNL Profile] る手順については、[ ユーザーガイド ](../ui/user-guide.md) を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[!DNL Real-Time Customer Profile] API の一部です。 先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## エクスポートジョブの作成

データ [!DNL Profile] 書き出すには、まずデータの書き出し先となるデータセットを作成してから、新しい書き出しジョブを開始する必要があります。 これらの手順は両方とも、Experience Platform API を使用して実行できます。前者は Catalog Service API を使用し、後者はリアルタイム顧客プロファイル API を使用します。 各手順を完了するための詳細な手順については、以下の節で説明しています。

### ターゲットデータセットの作成

データ [!DNL Profile] 書き出す場合は、まずターゲットデータセットを作成する必要があります。 データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の 1 つは、データセットのベースとなるスキーマ（以下の API サンプルリクエストの `schemaRef.id`）です。プロファイルデータを書き出すには、データセットが [!DNL XDM Individual Profile] 結合スキーマ（`https://ns.adobe.com/xdm/context/profile__union`）に基づいている必要があります。 結合スキーマは、同じクラスを共有するスキーマのフィールドを集約する、システム生成の読み取り専用スキーマです。 この場合、これは [!DNL XDM Individual Profile] クラスです。 和集合表示スキーマについて詳しくは、『スキーマ構成の基本ガイド』の [ 和集合の節 ](../../xdm/schema/composition.md#union) を参照してください。

このチュートリアルの手順では、[!DNL Catalog] API を使用して [!DNL XDM Individual Profile] 和集合スキーマを参照するデータセットを作成する方法の概要を説明します。 [!DNL Experience Platform] ユーザーインターフェイスを使用して、結合スキーマを参照するデータセットを作成することもできます。 UI の使用手順については、[ オーディエンスの書き出しについてのこの UI チュートリアル ](../../segmentation/tutorials/create-dataset-export-segment.md) で概要を説明していますが、こちらも適用できます。 完了したら、このチュートリアルに戻り、[新しいエクスポートジョブを開始する](#initiate)手順に進むことができます。

互換性のあるデータセットが既に存在し、その ID がわかっている場合は、[新しいエクスポートジョブを開始する](#initiate)手順に直接進むことができます。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

次のリクエストは、新しいデータセットを作成し、ペイロードに設定パラメーターを提供します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "Profile Data Export",
        "schemaRef": {
          "id": "https://ns.adobe.com/xdm/context/profile__union",
          "contentType": "application/vnd.adobe.xed+json;version=1"
        }
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | データセットのわかりやすい名前。 |
| `schemaRef.id` | データセットが関連付けられる和集合表示（スキーマ）の ID。 |

**応答** 

応答に成功した場合、新しく作成されたデータセットの読み取り専用のシステム生成された一意の ID を含む配列が返されます。プロファイルデータを正常にエクスポートするには、適切に設定されたデータセット ID が必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### エクスポートジョブの開始 {#initiate}

和集合を保持するデータセットが用意できたら、リアルタイム顧客プロファイル API の `/export/jobs` エンドポイントに対して POST リクエストを実行し、リクエストの本文に書き出すデータの詳細を指定することで、プロファイルデータをデータセットに保持する書き出しジョブを作成できます。

**API 形式**

```http
POST /export/jobs
```

**リクエスト**

次のリクエストは、新しいエクスポートジョブを作成し、ペイロードに設定パラメーターを提供します。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/export/jobs \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "fields": "identities.id,personalEmail.address",
    "mergePolicy": {
      "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
      "version": 1
    },
    "additionalFields": {
      "eventList": {
        "fields": "environment.browserDetails.name,environment.browserDetails.version",
        "filter": {
          "fromIngestTimestamp": "2018-10-25T13:22:04-07:00"
        }
      }
    },
    "destination": {
      "datasetId": "5b020a27e7040801dedba61b",
      "segmentPerBatch": false
    },
    "schema": {
      "name": "_xdm.context.profile"
    }
  }' 
```

| プロパティ | 説明 |
| -------- | ----------- |
| `fields` | *（オプション）*&#x200B;エクスポートに含めるデータフィールドを、このパラメーターで指定されたデータフィールドのみに制限します。この値を省略すると、エクスポートされるデータにすべてのフィールドが含まれます。 |
| `mergePolicy` | *（オプション）*&#x200B;エクスポートされるデータを管理する結合ポリシーを指定します。複数のオーディエンスが書き出される場合は、このパラメーターを含めます。 |
| `mergePolicy.id` | 結合ポリシーの ID。 |
| `mergePolicy.version` | 使用する結合ポリシーの特定のバージョンです。この値を省略すると、デフォルトで最新バージョンが使用されます。 |
| `additionalFields.eventList` | *（オプション）* 次の 1 つ以上の設定を指定して、子または関連するオブジェクト用に書き出される時系列イベントフィールドを制御します。<ul><li>`eventList.fields`：エクスポートするフィールドを制御します。</li><li>`eventList.filter`：関連オブジェクトから取得される結果を制限する基準を指定します。エクスポートに必要な最小値（通常は日付）が基準として予期されます。</li><li>`eventList.filter.fromIngestTimestamp`：時系列イベントを、指定されたタイムスタンプ以降に取り込まれたイベントにフィルターします。 これは、イベント時間自体ではなく、イベントの取得時間です。</li></ul> |
| `destination` | **（必須）**&#x200B;エクスポートするデータの宛先情報：<ul><li>`destination.datasetId`：**（必須）**&#x200B;データのエクスポート先のデータセットの ID。</li><li>`destination.segmentPerBatch`：*（オプション）*&#x200B;指定しない場合、ブール値はデフォルトで `false` になります。値 `false` は、すべてのセグメント定義 ID を 1 つのバッチ ID にエクスポートします。 値が `true` の場合、1 つのセグメント定義 ID を 1 つのバッチ ID にエクスポートします。 値を `true` に設定すると、バッチエクスポートのパフォーマンスに影響を与える場合があることに注意してください。</li></ul> |
| `schema.name` | **（必須）**&#x200B;データのエクスポート先のデータセットに関連付けられているスキーマの名前。 |

>[!NOTE]
>
>プロファイルデータのみをエクスポートし、関連する時系列データを含めない場合は、「additionalFields」オブジェクトをリクエストから削除します。

**応答** 

成功した応答では、リクエストで指定したプロファイルデータが含まれるデータセットが返されます。

```json
{
    "profileInstanceId": "ups",
    "jobType": "BATCH",
    "id": 24115,
    "schema": {
        "name": "_xdm.context.profile"
    },
    "mergePolicy": {
        "id": "0bf16e61-90e9-4204-b8fa-ad250360957b",
        "version": 1
    },
    "status": "NEW",
    "requestId": "IwkVcD4RupdSmX376OBVORvcvTdA4ypN",
    "computeGatewayJobId": {},
    "metrics": {
        "totalTime": {
            "startTimeInMs": 1559674261657
        }
    },
    "destination": {
      "dataSetId": "5cf6bcf79ecc7c14530fe436",
      "segmentPerBatch": false,
      "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
    },
    "updateTime": 1559674261868,
    "imsOrgId": "{ORG_ID}",
    "creationTime": 1559674261657
}
```

## すべてのエクスポートジョブのリスト

`export/jobs` エンドポイントにGET リクエストを実行することで、特定の組織のすべての書き出しジョブのリストを返すことができます。 リクエストは、以下に示すように、クエリパラメーター `limit` および `offset` もサポートします。

**API 形式**

```http
GET /export/jobs
GET /export/jobs?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `start` | リクエストの作成時刻に従って、返された結果のページをオフセットします。例：`start=4` |
| `limit` | 返す結果の数を制限します。例：`limit=10` |
| `page` | リクエストの作成時刻に従って、特定のページの結果を返します。例：`page=2` |
| `sort` | 特定のフィールドを基準に、昇順（**`asc`**）または降順（**`desc`**）で結果を並べ替えます。 結果の複数ページを返す場合、並べ替えパラメーターは機能しません。例：`sort=updateTime:asc` |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs/ \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

応答には、組織で作成されたエクスポートジョブを含む `records` オブジェクトが含まれています。

```json
{
  "records": [
    {
      "profileInstanceId": "ups",
      "jobType": "BATCH",
      "id": 726,
      "schema": {
          "name": "_xdm.context.profile"
      },
      "mergePolicy": {
          "id": "timestampOrdered-none-mp",
          "version": 1
      },
      "status": "SUCCEEDED",
      "requestId": "d995479c-8a08-4240-903b-af469c67be1f",
      "computeGatewayJobId": {
          "exportJob": "f3058161-7349-4ca9-807d-212cee2c2e94",
          "pushJob": "feaeca05-d137-4605-aa4e-21d19d801fc6"
      },
      "metrics": {
          "totalTime": {
              "startTimeInMs": 1538615973895,
              "endTimeInMs": 1538616233239,
              "totalTimeInMs": 259344
          },
          "profileExportTime": {
              "startTimeInMs": 1538616067445,
              "endTimeInMs": 1538616139576,
              "totalTimeInMs": 72131
          },
          "aCPDatasetWriteTime": {
              "startTimeInMs": 1538616195172,
              "endTimeInMs": 1538616195715,
              "totalTimeInMs": 543
          }
      },
      "destination": {
          "datasetId": "5b7c86968f7b6501e21ba9df",
          "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
      },
      "updateTime": 1538616233239,
      "imsOrgId": "{ORG_ID}",
      "creationTime": 1538615973895
    },
    {
      "profileInstanceId": "test_xdm_latest_profile_20_e2e_1538573005395",
      "errors": [
        {
          "code": "0090000009",
          "msg": "Error writing profiles to output path 'adl://va7devprofilesnapshot.azuredatalakestore.net/snapshot/722'",
          "callStack": "com.adobe.aep.unifiedprofile.common.logging.Logger" 
        },
        {
          "code": "unknown",
          "msg": "Job aborted.",
          "callStack": "org.apache.spark.SparkException: Job aborted."
        }
      ],
      "jobType": "BATCH",
      "filter": {
        "segments": [
            {
                "segmentId": "7a93d2ff-a220-4bae-9a4e-5f3c35032be3"
            }
        ]
      },
      "id": 722,
      "schema": {
          "name": "_xdm.context.profile"
      },
      "mergePolicy": {
          "id": "7972e3d6-96ea-4ece-9627-cbfd62709c5d",
          "version": 1
      },
      "status": "FAILED",
      "requestId": "KbOAsV7HXmdg262lc4yZZhoml27UWXPZ",
      "computeGatewayJobId": {
          "exportJob": "15971e0f-317c-4390-9038-1a0498eb356f"
      },
      "metrics": {
          "totalTime": {
              "startTimeInMs": 1538573416687,
              "endTimeInMs": 1538573922551,
              "totalTimeInMs": 505864
          },
          "profileExportTime": {
              "startTimeInMs": 1538573872211,
              "endTimeInMs": 1538573918809,
              "totalTimeInMs": 46598
          }
      },
      "destination": {
          "datasetId": "5bb4c46757920712f924a3eb",
          "batchId": ""
      },
      "updateTime": 1538573922551,
      "imsOrgId": "{ORG_ID}",
      "creationTime": 1538573416687
    }
  ],
  "page": {
      "sortField": "createdTime",
      "sort": "desc",
      "pageOffset": "1538573416687_722",
      "pageSize": 2
  },
  "link": {
      "next": "/export/jobs/?limit=2&offset=1538573416687_722"
  }
}
```

## エクスポートの進行状況の監視

特定のエクスポートジョブの詳細を表示したり、処理中のステータスを監視したりするには、`/export/jobs` エンドポイントに対して GET リクエストを実行し、エクスポートジョブの `id` をパスを含めます。`status` フィールドによって値「SUCCEEDED」が返されると、エクスポートジョブが完了します。

**API 形式**

```http
GET /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセスするエクスポートジョブの `id`。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs/24115 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```json
{
    "profileInstanceId": "ups",
    "jobType": "BATCH",
    "id": 24115,
    "schema": {
        "name": "_xdm.context.profile"
    },
    "mergePolicy": {
        "id": "0bf16e61-90e9-4204-b8fa-ad250360957b",
        "version": 1
    },
    "status": "SUCCEEDED",
    "requestId": "YwMt1H8QbVlGT2pzyxgwFHTwzpMbHrTq",
    "computeGatewayJobId": {
      "exportJob": "305a2e5c-2cf3-4746-9b3d-3c5af0437754",
      "pushJob": "963f275e-91a3-4fa1-8417-d2ca00b16a8a"
    },
    "metrics": {
      "totalTime": {
        "startTimeInMs": 1547053539564,
        "endTimeInMs": 1547054743929,
        "totalTimeInMs": 1204365
      },
      "profileExportTime": {
        "startTimeInMs": 1547053667591,
        "endTimeInMs": 1547053778195,
        "totalTimeInMs": 110604
      },
      "aCPDatasetWriteTime": {
        "startTimeInMs": 1547054660416,
        "endTimeInMs": 1547054698918,
        "totalTimeInMs": 38502
      }
    },
    "destination": {
      "dataSetId": "5cf6bcf79ecc7c14530fe436",
      "segmentPerBatch": false,
      "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
    },
    "updateTime": 1559674261868,
    "imsOrgId": "{ORG_ID}",
    "creationTime": 1559674261657
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `batchId` | 成功したエクスポートから作成されるバッチの識別子。プロファイルデータを読み取る際に参照目的で使用されます。 |

## エクスポートジョブのキャンセル

Experience Platform では、既存のエクスポートジョブをキャンセルできます。この機能は、エクスポートジョブが完了しなかったか、処理段階で停止した場合など、様々な理由で役立つ場合があります。エクスポートジョブをキャンセルするには、`/export/jobs` エンドポイントに対して DELETE リクエストを実行し、キャンセルするエクスポートジョブの `id` をリクエストパスに含めます。

**API 形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセスするエクスポートジョブの `id`。 |

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/export/jobs/726 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 204（コンテンツなし）と空の応答本文が返され、キャンセル操作が成功したことを示します。

## 次の手順

エクスポートが正常に完了すると、データは Experience Platform のデータレイク内で使用できます。その後、エクスポートに関連付けられた `batchId` を使用して、[データアクセス API](https://www.adobe.io/experience-platform-apis/references/data-access/) でデータにアクセスできます。エクスポートのサイズに応じて、データがチャンクに格納され、バッチが複数のファイルで構成される場合があります。

データアクセス API を使用してバッチファイルにアクセスしてダウンロードする手順については、[データアクセスのチュートリアル](../../data-access/tutorials/dataset-data.md)を参照してください。

また、Adobe Experience Platform クエリサービスを使用して、正常に書き出されたリアルタイム顧客プロファイルデータにアクセスすることもできます。 クエリサービスでは、UI または RESTful API を使用して、クエリの書き込みと検証を行い、データレイク内のデータに対してクエリを実行することができます。

オーディエンスデータに対してクエリを実行する方法ついて詳しくは、[クエリサービスのドキュメント](../../query-service/home.md)を参照してください。

## 付録

次の節では、Profile API の書き出しジョブに関する追加情報を示します。

### その他のエクスポートペイロードの例

[ 書き出しジョブの開始 ](#initiate) の節で示した API 呼び出しの例では、プロファイル（レコード）とイベント（時系列）の両方のデータを含むジョブを作成します。 この節では、エクスポートするデータ型を 1 つのみに制限する、追加のリクエストペイロードの例を示します。

次のペイロードは、プロファイルデータのみを含み（イベントを含まない）、書き出しジョブを作成します。

```json
{
    "fields": "identities.id,personalEmail.address",
    "mergePolicy": {
      "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
      "version": 1
    },
    "destination": {
      "datasetId": "5b020a27e7040801dedba61b",
      "segmentPerBatch": false
    },
    "schema": {
      "name": "_xdm.context.profile"
    }
  }
```

イベントデータのみを含む（プロファイル属性を含まない）書き出しジョブを作成するには、ペイロードは次のようになります。

```json
{
    "fields": "identityMap",
    "mergePolicy": {
      "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
      "version": 1
    },
    "additionalFields": {
      "eventList": {
        "fields": "environment.browserDetails.name,environment.browserDetails.version",
        "filter": {
          "fromIngestTimestamp": "2018-10-25T13:22:04-07:00"
        }
      }
    },
    "destination": {
      "datasetId": "5b020a27e7040801dedba61b",
      "segmentPerBatch": false
    },
    "schema": {
      "name": "_xdm.context.profile"
    }
  }
```

### オーディエンスの書き出し

また、書き出しジョブエンドポイントを使用して、データの代わりにオーディエンス [!DNL Profile] 書き出すこともできます。 詳しくは、[Segmentation API での書き出しジョブ ](../../segmentation/api/export-jobs.md) に関するガイドを参照してください。
