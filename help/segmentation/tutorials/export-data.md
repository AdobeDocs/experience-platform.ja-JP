---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したデータの書き出し
topic: tutorial
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '1953'
ht-degree: 1%

---


# APIを使用したプロファイルデータの書き出し

リアルタイム顧客プロファイルでは、属性データと行動データの両方を含む複数のソースからのデータを統合することで、個々の顧客の単一の表示を構築できます。 プロファイル内のデータをデータセットにエクスポートして、さらに処理することができます。 このチュートリアルでは、 [セグメント化APIを使用して書き出しジョブを作成し管理する手順を順を追って説明します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)。

書き出しジョブの作成に加えて、プロファイルアクセスAPIを使用したり、投影を通じてプロファイルデータにアクセスすることもできます。 これらのその他のアクセスパターンについて詳しくは、 [プロファイルアクセスAPIのチュートリアル](../../profile/api/entities.md) 、またはエッジの宛先と投影法の [](../../profile/api/edge-projections.md) 設定に関するチュートリアルを参照してください。

## はじめに

このチュートリアルでは、プロファイルデータの操作に関連する様々なAdobe Experience Platformサービスについて、十分な理解が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [Adobe Experience Platformセグメントサービス](../home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [Experience Data Model(XDM)](../../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

### 必須ヘッダー

また、PlatformAPIの呼び出しを正しく行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があります。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへの要求には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

すべてのPOST、PUT、およびPATCHリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## 書き出しジョブの作成

プロファイルデータをエクスポートするには、まずデータのエクスポート先となるデータセットを作成し、新しいエクスポートジョブを開始する必要があります。 これらの手順はどちらも、Experience PlatformAPIを使用して行うことができます。前者はCatalog Service APIを使用し、後者はReal-time Customer Commentation APIを使用します。 各手順の詳細な手順については、以降の節で説明します。

- [ターゲットデータセットの作成](#create-a-target-dataset) — 書き出したデータを保存するデータセットを作成します。
- [新しいエクスポートジョブの開始](#initiate-export-job) - XDM個別プロファイルデータをデータセットに入力します。

### ターゲットデータセットの作成

プロファイルデータを書き出す場合は、まずターゲットデータセットを作成する必要があります。 データセットを正しく設定して、エクスポートが正常に完了するようにすることが重要です。

重要な考慮事項の1つは、データセットのベースとなるスキーマです(以下のAPIサンプルリクエスト`schemaRef.id` を参照)。 セグメントをエクスポートするには、XDM個別プロファイル和集合スキーマ(`https://ns.adobe.com/xdm/context/profile__union`)に基づくデータセットが必要です。 和集合スキーマは、システム生成の読み取り専用スキーマで、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するスキーマのフィールドを集計します。 和集合表示のスキーマの詳細については、『スキーマレジストリ開発ガイド』の「 [リアルタイム顧客プロファイル」の節を参照してください](../../xdm/schema/composition.md#union)。

このチュートリアルでは、Catalog APIを使用してXDM Individualプロファイル和集合スキーマを参照するデータセットを作成する方法について説明します。 また、Adobe Experience Platformのユーザーインターフェイスを使用して、和集合スキーマを参照するデータセットを作成することもできます。 UIの使用手順は、セグメントの書き出しに関する [このUIチュートリアルで説明しています](./create-dataset-export-segment.md) 。ここでも説明します。 完了したら、このチュートリアルに戻って、新しい書き出しジョブを [開始する手順に進むことができます](#initiate-export-job)。

互換性のあるデータセットが既に存在し、そのIDがわかっている場合は、新しい書き出しジョブを [開始するための手順に直接進むことができます](#initiate-export-job)。

**API形式**

```http
POST /dataSets
```

**リクエスト**

次のリクエストは、ペイロードに設定パラメーターを提供する新しいデータセットを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "Profile Data Export",
        "schemaRef": {
          "id": "https://ns.adobe.com/xdm/context/profile__union",
          "contentType": "application/vnd.adobe.xed+json;version=1"
        },
        "fileDescription": {
          "persisted": true,
          "containerFormat": "parquet",
          "format": "parquet"
        }
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | データセットを説明する名前。 |
| `schemaRef.id` | データセットが関連付けられる和集合表示(スキーマ)のID。 |
| `fileDescription.persisted` | に設定した場合、和集合セットが表示内で保持され `true`るようにするBoolean値です。 |

**応答**

正常に完了すると、新たに作成されたデータセットの読み取り専用、システム生成、一意のIDを含む配列が返されます。 プロファイルデータを正常にエクスポートするには、適切に設定されたデータセットIDが必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### 書き出しジョブの開始

和集合持続性データセットを取得したら、リアルタイム顧客プロファイルAPIのエンドポイントにPOSTリクエストを送信し、リクエストの本文でエクスポートするデータの詳細を提供することで、プロファイルデータをデータセットに永続化するエクスポートジョブを作成できます。 `/export/jobs`

**API形式**

```http
POST /export/jobs
```

**リクエスト**

次のリクエストは、ペイロードに設定パラメーターを提供する、新しい書き出しジョブを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/export/jobs \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "fields": "identities.id,personalEmail.address",
    "mergePolicy": {
      "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
      "version": 1
    },
    "filter": {
      "segments": [
        {
          "segmentId": "4edc8488-2c35-4f6d-b4c6-9075c68d2df4",
          "segmentNs": "AAM",
          "status": ["realized"]
        },
        {
          "segmentId": "1rfe8422-334d-12f4-3sd4-12cf6g990g51",
          "segmentNs": "UPS",
          "status": ["exited"]
        }
      ],
      "segmentQualificationTime": {
            "startTime": "2019-09-01T00:00:00Z",
            "endTime": "2019-09-02T00:00:00Z"
        },
      "fromIngestTimestamp": "2018-10-25T13:22:04-07:00",
      "emptyProfiles": false
    },
    "additionalFields" : {
      "eventList": {
        "fields": "environment.browserDetails.name,environment.browserDetails.version",
        "filter": {
          "fromIngestTimestamp": "2018-10-25T13:22:04-07:00"
        }
      }
    },
    "destination": {
      "datasetId": "5b020a27e7040801dedba61b",
      "segmentPerBatch": true
    },
    "schema": {
      "name": "_xdm.context.profile"
    },
    "evaluationInfo": {
        "segmentation": true
    }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `fields` | *（オプション）* 、エクスポートに含めるデータフィールドを、このパラメーターで指定されたデータフィールドのみに制限します。 セグメントの作成時にも同じパラメーターが使用できるので、セグメント内のフィールドが既にフィルターされている可能性があります。 この値を省略すると、書き出されたデータにすべてのフィールドが含まれます。 |
| `mergePolicy` | *（オプション）* 、エクスポートされたデータを管理するマージポリシーを指定します。 複数のセグメントを書き出す場合は、このパラメーターを含めます。 この値を省略すると、Export Serviceで、セグメントによって提供されるマージポリシーが使用されます。 |
| `mergePolicy.id` | マージポリシーのID。 |
| `mergePolicy.version` | 使用するマージポリシーの特定のバージョンです。 この値を省略すると、デフォルトで最新バージョンが使用されます。 |
| `filter` | *（オプション）* 次の1つ以上のフィルターを指定して、エクスポートの前にセグメントに適用します。 |
| `filter.segments` | *（オプション）* 書き出すセグメントを指定します。 この値を省略すると、すべてのプロファイルのすべてのデータがエクスポートされます。 次のフィールドを含むセグメントオブジェクトの配列を受け入れます。<ul><li>`segmentId`: **(使用する場合は必須`segments`)** プロファイルのセグメントIDをエクスポートします。</li><li>`segmentNs` *（オプション）* 指定したのセグメント名前空間 `segmentID`。</li><li>`status` *（オプション）* 、のステータスフィルタを提供する文字列の配列 `segmentID`。 デフォルトでは、 `status` は、現在の時間にセグメントに含まれるすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。 使用できる値は次のとおりです。 `"realized"`、 `"existing"`および `"exited"`。</br></br>詳しくは、「セグメントの [作成」チュートリアルを参照してください](./create-a-segment.md)。</li></ul> |
| `filter.segmentQualificationTime` | *（オプション）* 、セグメント資格時間に基づいてフィルターします。 開始時間及び/又は終了時間を提供することができる。 |
| `filter.segmentQualificationTime.startTime` | *（オプション）* 特定のステータスのセグメントIDに対するセグメント資格開始時間。 これは指定されていません。セグメントID資格の開始時間に対するフィルターはありません。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.segmentQualificationTime.endTime` | *（オプション）* 特定のステータスのセグメントIDに対するセグメントクオリフィケーションの終了時間。 これは指定されていません。セグメントID資格の終了時間にフィルターは適用されません。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp ` | *（オプション）* 、書き出すプロファイルに、このタイムスタンプの後に更新されたアイテムのみが含まれるように制限します。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 <ul><li>`fromIngestTimestamp` を **プロファイルに対して設定する場合**、次の条件を満たします。 マージされた更新されたプロファイルが指定されたタイムスタンプより大きい場合に、マージされたすべてのタイムスタンプを含めます。 オペランドをサポート `greater_than` します。</li><li>`fromTimestamp` の **イベント**: このタイムスタンプの後に取り込まれるすべてのイベントは、結果のプロファイル結果に応じて書き出されます。 これは、イベント時間そのものではなく、イベントの取り込み時間です。</li> |
| `filter.emptyProfiles` | *（オプション）* Boolean。 プロファイルには、プロファイルレコード、ExperienceEventレコード、またはその両方を含めることができます。 プロファイルレコードがなく、ExperienceEventレコードのみのプロファイルは、「emptyProfiles」と呼ばれます。 プロファイルストア内のすべてのプロファイル（「emptyProfiles」も含む）を書き出すには、の値をに設定 `emptyProfiles` し `true`ます。 をに設定 `emptyProfiles``false`すると、ストア内のプロファイルレコードを持つプロファイルのみがエクスポートされます。 デフォルトでは、 `emptyProfiles` 属性が含まれていない場合、プロファイルレコードを含むプロファイルのみがエクスポートされます。 |
| `additionalFields.eventList` | *（オプション）* 次の設定を1つ以上指定して、子オブジェクトまたは関連付けられたオブジェクト用に書き出す時系列イベントフィールドを制御します。<ul><li>`eventList.fields`: 書き出すフィールドを制御します。</li><li>`eventList.filter`: 関連するオブジェクトから含まれる結果を制限する基準を指定します。 エクスポートに必要な最小値（通常は日付）。</li><li>`eventList.filter.fromIngestTimestamp`: 指定されたタイムスタンプの後に取り込まれたものに対するフィルター時系列イベント。 これは、イベント時間そのものではなく、イベントの取り込み時間です。</li></ul> |
| `destination` | **（必須）** 書き出すデータの保存先情報：<ul><li>`destination.datasetId`: **（必須）** 、データをエクスポートするデータセットのID。</li><li>`destination.segmentPerBatch`: *（オプション）* Boolean値。指定しなかった場合のデフォルト値は `false`です。 値を指定すると、すべてのセグメントIDが1つのバッチIDに `false` エクスポートされます。 値として、1つのセグメントIDを1つのバッチIDに `true` エクスポートします。 この値を設定すると、バッチエクスポートのパフォーマンスに影響を与える `true` 場合があります。</li></ul> |
| `schema.name` | **（必須）** 、データをエクスポートするデータセットに関連付けられているスキーマの名前。 |
| `evaluationInfo.segmentation` | *（オプション）* boolean値。指定しなかった場合のデフォルト値は `false`です。 値がの場合、セグメント化は書き出しジョブで行う必要があります。 `true` |

>[!NOTE]
>
>プロファイルデータのみを書き出し、関連するExperienceEventデータを含めない場合は、「additionalFields」オブジェクトをリクエストから削除します。

**応答**

成功した場合は、リクエストで指定されたプロファイルデータが入力されたデータセットが返されます。

```json
{
    "profileInstanceId": "ups",
    "jobType": "BATCH",
    "filter": {
      "segments": [
        {
          "segmentId": "4edc8488-2c35-4f6d-b4c6-9075c68d2df4",
          "segmentNs": "AAM",
          "status": ["realized"]
        },
        {
          "segmentId": "1rfe8422-334d-12f4-3sd4-12cf6g990g51",
          "segmentNs": "UPS",
          "status": ["exited"]
        }
      ]
    },
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
      "dataSetId" : "5cf6bcf79ecc7c14530fe436",
      "segmentPerBatch": true,
      "batches" : [
        {
          "segmentId": "4edc8488-2c35-4f6d-b4c6-9075c68d2df4",
          "segmentNs": "AAM",
          "status": ["realized"],
          "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
        },
        {
          "segmentId": "1rfe8422-334d-12f4-3sd4-12cf6g990g51",
          "segmentNs": "UPS",
          "status": ["exited"],
          "batchId": "df4gssdfb93a09f7e37fa53ad52"
        }
      ]
    },
    "updateTime": 1559674261868,
    "imsOrgId": "{IMS_ORG}",
    "creationTime": 1559674261657
}
```

リクエストに含ま `destination.segmentPerBatch` れていない場合(存在しない場合はデフォルトで `false`)、または値がに設定されていた場合 `false`、上記の応答内の `destination` オブジェクトは `batches``batchId`配列を持たず、1つだけ含めます（下図を参照）。 この1つのバッチにはすべてのセグメントIDが含まれますが、上の応答にはバッチIDごとに1つのセグメントIDが表示されます。

```json
  "destination": {
    "datasetId": "5cf6bcf79ecc7c14530fe436",
    "segmentPerBatch": false,
    "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
  }
```

## すべての書き出しジョブのリスト

エンドポイントに対してGET要求を実行すると、特定のIMS組織のすべての書き出しジョブのリストを返すことができ `export/jobs` ます。 このリクエストでは、以下に示すように、クエリパラメーター `limit` と `offset`もサポートされます。

**API形式**

```http
GET /export/jobs
GET /export/jobs?limit=4
GET /export/jobs?offset=2
```

| プロパティ | 説明 |
| -------- | ----------- |
| `limit` | 返すレコード数を指定します。 |
| `offset` | 指定した数値で返される結果のページをオフセットします。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/export/jobs/ \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -H 'x-sandbox-name: {SANDBOX_NAME}' 
```

**応答**

応答には、IMS組織によって作成された書き出しジョブを含む `records` オブジェクトが含まれます。

```json
{
  "records": [
    {
      "profileInstanceId": "ups",
      "jobType": "BATCH",
      "filter": {
          "segments": [
              {
                  "segmentId": "52c26d0d-45f2-47a2-ab30-ed06abc981ff"
              }
          ]
      },
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
      "imsOrgId": "{IMS_ORG}",
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
      "imsOrgId": "{IMS_ORG}",
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

## 書き出しの進行状況の監視

特定の書き出しジョブの詳細を表示したり、処理中にそのステータスを監視したりするには、 `/export/jobs` エンドポイントにGETリクエストを行い、書き出しジョブ `id` のパスを含めます。 エクスポートジョブは、 `status` フィールドが値「SUCCEEDEDED」を返すと完了します。

**API形式**

```http
GET /export/jobs/{EXPORT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセス `id` する書き出しジョブの名前。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/export/jobs/24115 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```json
{
    "profileInstanceId": "ups",
    "jobType": "BATCH",
    "filter": {
      "segments": [
        {
          "segmentId": "4edc8488-2c35-4f6d-b4c6-9075c68d2df4",
          "segmentNs": "AAM",
          "status": ["realized"]
        },
        {
          "segmentId": "1rfe8422-334d-12f4-3sd4-12cf6g990g51",
          "segmentNs": "UPS",
          "status": ["exited"]
        }
      ]
    },
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
      "dataSetId" : "5cf6bcf79ecc7c14530fe436",
      "segmentPerBatch": true,
      "batches" : [
        {
          "segmentId": "4edc8488-2c35-4f6d-b4c6-9075c68d2df4",
          "segmentNs": "AAM",
          "status": ["realized"],
          "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
        },
        {
          "segmentId": "1rfe8422-334d-12f4-3sd4-12cf6g990g51",
          "segmentNs": "UPS",
          "status": ["exited"],
          "batchId": "df4gssdfb93a09f7e37fa53ad52"
        }
      ]
    },
    "updateTime": 1559674261868,
    "imsOrgId": "{IMS_ORG}",
    "creationTime": 1559674261657
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `batchId` | プロファイルデータを読み取る際に参照用に使用される、成功したエクスポートから作成されたバッチの識別子。 |

## 書き出しジョブのキャンセル

Experience Platformを使用すると、既存の書き出しジョブをキャンセルできます。これは、書き出しジョブが完了しなかったか、処理段階で停止した場合など、様々な理由で役立ちます。 書き出しジョブをキャンセルするには、エンドポイントに対してDELETE要求を実行し、 `/export/jobs` キャンセルする書き出しジョブ `id` を要求パスに含めます。

**API形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセス `id` する書き出しジョブの名前。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/export/jobs/726 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

削除要求が成功すると、HTTPステータス204（コンテンツなし）と空の応答本文が返され、キャンセル操作が成功したことを示します。

## 次の手順

エクスポートが正常に完了すると、Experience Platformのデータレーク内でデータを使用できます。 その後、 [データアクセスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) (Data Access API)を使用して `batchId` 、エクスポートに関連付けられたを使用してデータにアクセスできます。 エクスポートのサイズに応じて、データはチャンクになり、バッチは複数のファイルで構成される場合があります。

Data Access APIを使用してバッチファイルにアクセスし、ダウンロードする手順については、「 [データアクセス](../../data-access/tutorials/dataset-data.md)」チュートリアルに従ってください。

また、Adobe Experience Platformクエリサービスを使用して、正常にエクスポートされたリアルタイム顧客プロファイルデータにアクセスすることもできます。 クエリサービスでは、UIまたはRESTful APIを使用して、データレーク内のデータに対してクエリの書き込み、検証、および実行を行うことができます。

オーディエンスデータのクエリ方法の詳細については、 [クエリサービスのドキュメントを参照してください](../../query-service/home.md)。