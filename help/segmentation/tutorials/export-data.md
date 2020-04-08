---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したデータの書き出し
topic: tutorial
translation-type: tm+mt
source-git-commit: 409d98818888f2758258441ea2d993ced48caf9a

---


# APIを使用したプロファイルデータの書き出し

リアルタイム顧客プロファイルを使用すると、属性データと行動データの両方を含む複数のソースからのデータを統合することで、個々の顧客の単一の表示を構築できます。 その後、プロファイル内で利用できるデータをデータセットにエクスポートして、さらに処理することができます。 このチュートリアルでは、セグメント化APIを使用して書き出しジョブを作成および管理する手順を順を追って [説明します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)。

書き出しジョブの作成に加えて、プロファイルアクセスAPIを使用して、および投影を通じてプロファイルデータにアクセスすることもできます。 その他のアクセスパターンについて詳しくは、 [プロファイルアクセスAPIのチュートリアル](../../profile/api/entities.md) 、またはエッジの宛先と投影 [の設定に関するチュートリアルを参照してください](../../profile/api/edge-projections.md) 。

## はじめに

このチュートリアルでは、プロファイルデータの操作に関わる様々なAdobe Experience Platformサービスについて理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたプロファイルをリアルタイムで提供します。
- [Adobe Experience Platform Segmentation Service](../home.md):リアルタイム顧客オーディエンスデータから顧客セグメントを作成できます。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

### 必須ヘッダ

また、このチュートリアルでは、プラットフォームAPIの呼び出しを正 [常に行うために](../../tutorials/authentication.md) 、認証のチュートリアルを完了している必要があります。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへの要求には、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

すべてのPOST、PUTおよびPATCHリクエストには、次の追加のヘッダーが必要です。

- コンテンツタイプ：application/json

## 書き出しジョブの作成

プロファイルデータを書き出すには、まずデータの書き出し先のデータセットを作成し、次に新しい書き出しジョブを開始する必要があります。 これらの手順はどちらも、Experience Platform APIを使用して実行できます。前者はCatalog Service APIを使用し、後者はReal-time CustomerプロファイルAPIを使用します。 各手順を実行する詳細な手順については、以降の節で説明します。

- [ターゲットデータセットの作成](#create-a-target-dataset) — 書き出したデータを保持するデータセットを作成します。
- [新しい書き出しジョブの開始](#initiate-export-job) — データセットにXDM個々のジョブデータを入力します。

### ターゲットデータセットの作成

プロファイルデータを書き出す場合は、まずターゲットデータセットを作成する必要があります。 データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の1つは、データセットのベースとなるスキーマ`schemaRef.id` （以下のAPIサンプルリクエスト）です。 セグメントを書き出すには、XDM個々のプロファイル和集合スキーマ(`https://ns.adobe.com/xdm/context/profile__union`)に基づくデータセットが必要です。 和集合スキーマは、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するスキーマのフィールドを集計する、システム生成の読み取り専用のスキーマです。 和集合表示スキーマの詳細については、『スキーマレジストリ開発 [者ガイド』の「リアルタイム顧客プロファイル」の節を参照してください](../../xdm/schema/composition.md#union)。

このチュートリアルでは、Catalog APIを使用してXDM個々のプロファイル和集合スキーマを参照するデータセットを作成する方法について説明します。 また、Adobe Experience Platformのユーザーインターフェイスを使用して、ユーザースキーマを参照するデータセットを作成することもできます。 UIの使用手順は、このUIチュートリアルでセグメ [ントの書き出しについて説明します](./create-dataset-export-segment.md) が、ここでも説明します。 完了したら、このチュートリアルに戻り、新しい書き出しジョブを開始す [る手順に進むことができます](#initiate-export-job)。

互換性のあるデータセットが既に存在し、そのIDがわかっている場合は、新しい書き出しジョブを開始す [る手順に直接進むことができます](#initiate-export-job)。

**API形式**

```http
POST /dataSets
```

**リクエスト**

次のリクエストでは、ペイロードに設定パラメーターを提供する新しいデータセットを作成します。

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
| `fileDescription.persisted` | に設定した場合、データセットが `true`和集合表示に保持されます。 |

**応答**

成功した場合、新しく作成されたデータセットの読み取り専用の、システム生成の一意のIDを含む配列が返されます。 データセットデータを正常に書き出すには、適切に設定されたプロファイルセットIDが必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### 書き出しジョブの開始

和集合永続的なデータセットを取得したら、Real-time Customer Commenter APIのエンドポイントにPOSTリクエストを作成し、リクエストの本文で書き出すデータの詳細を提供することで、プロファイルデータをデータセットに永続化する書き出しジョブを作成できます。 `/export/jobs`

**API形式**

```http
POST /export/jobs
```

**リクエスト**

次のリクエストは、ペイロードに設定パラメーターを提供する新しい書き出しジョブを作成します。

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
    }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `fields` | *（オプション）* 、エクスポートに含めるデータフィールドを、このパラメーターで指定されたデータフィールドのみに制限します。 セグメントの作成時にも同じパラメータが使用できるので、セグメント内のフィールドが既にフィルタされている可能性があります。 この値を省略すると、書き出されたデータにすべてのフィールドが含まれます。 |
| `mergePolicy` | *（オプション）* 、エクスポートされたデータを管理するマージポリシーを指定します。 複数のセグメントが書き出される場合は、このパラメータを含めます。 この値を省略すると、エクスポートサービスでセグメントが提供するマージポリシーが使用されます。 |
| `mergePolicy.id` | マージポリシーのID。 |
| `mergePolicy.version` | 使用するマージポリシーの特定のバージョンです。 この値を省略すると、デフォルトで最新バージョンが使用されます。 |
| `filter` | *（オプション）* ：エクスポートの前にセグメントに適用する次のフィルターを1つ以上指定します。 |
| `filter.segments` | *（オプション）* 、書き出すセグメントを指定します。 この値を省略すると、すべてのデータが書き出されるプロファイルになります。 次のフィールドを含むセグメントオブジェクトの配列を受け入れます。<ul><li>`segmentId`: **(使用する場合は必`segments`須)** 、エクスポートするプロファイルのセグメントID。</li><li>`segmentNs` *（オプション）* 、指定したのセグメント名前空間 `segmentID`。</li><li>`status` *（オプション）* 、のステータスフィルターを提供する文字列の配 `segmentID`列。 デフォルトでは、 `status` は、現在の時間にセ `["realized", "existing"]` グメントに含まれるすべてのプロファイルを表す値を持ちます。 次の値を指定できます。 `"realized"`、、 `"existing"`および `"exited"`。</br></br>詳しくは、「セグメントの作成」チュートリ [アルを参照してくださ](./create-a-segment.md)い。</li></ul> |
| `filter.segmentQualificationTime` | *（オプション）* 、セグメントの資格時間に基づいてフィルターします。 開始時間及び/又は終了時間を提供する。 |
| `filter.segmentQualificationTime.startTime` | *（オプション）* 、特定のステータスのセグメントIDのセグメント資格開始時間。 このオプションは指定されていないので、セグメントIDの開始時間に対するフィルターは適用されません。 タイムスタンプは [RFC 3339形式で提供する必要があります](https://tools.ietf.org/html/rfc3339) 。 |
| `filter.segmentQualificationTime.endTime` | *（オプション）* 、特定のステータスのセグメントIDのセグメントクオリフィケーション終了時間。 このオプションは指定されていないので、終了時間にセグメントIDの資格に対するフィルターは適用されません。 タイムスタンプは [RFC 3339形式で提供する必要があります](https://tools.ietf.org/html/rfc3339) 。 |
| `filter.fromIngestTimestamp ` | *（オプション）* 、書き出したプロファイルには、このタイムスタンプの後に更新されたアイテムのみが含まれるように制限します。 タイムスタンプは [RFC 3339形式で提供する必要があります](https://tools.ietf.org/html/rfc3339) 。 <ul><li>`fromIngestTimestamp` の場合 **、**&#x200B;プロファイルの場合：結合された更新されたプロファイルが指定されたタイムスタンプより大きい場合、結合されたすべてのタイムスタンプを含めます。 オペランド `greater_than` をサポート</li><li>`fromTimestamp` イベント ****:このイベントタイムスタンプの後に取り込まれるすべてのタイムスタンプは、結果のタイムスタンプの結果に対応してプロファイルされます。 これは、イベント時間自体ではなく、インジェスト時間のイベントです。</li> |
| `filter.emptyProfiles` | *（オプション）* Boolean。 プロファイルには、プロファイルレコード、ExperienceEventレコード、またはその両方を含めることができます。 プロファイルレコードがなく、ExperienceEventレコードのみが含まれるプロファイルは、「emptyProfiles」と呼ばれます。 「emptyProfiles」を含む、プロファイルストア内のすべてのプロファイルを書き出すには、の値をに設定 `emptyProfiles` します `true`。 をに設定 `emptyProfiles` すると、スト `false`ア内のプロファイルレコードを持つプロファイルのみが書き出されます。 デフォルトでは、属性が含ま `emptyProfiles` れていない場合、プロファイルレコードを含むプロファイルのみが書き出されます。 |
| `additionalFields.eventList` | *（オプション）* 次の1つ以上の設定を指定して、子オブジェクトまたは関連オブジェクト用に書き出す時系列イベントフィールドを制御します。<ul><li>`eventList.fields`:書き出すフィールドを制御します。</li><li>`eventList.filter`:関連オブジェクトから含まれる結果を制限する基準を指定します。 エクスポートに必要な最小値（通常は日付）。</li><li>`eventList.filter.fromIngestTimestamp`:フィルターの時系列イベントは、指定されたタイムスタンプの後に取り込まれたものに対して使用されます。 これは、イベント時間自体ではなく、インジェスト時間のイベントです。</li></ul> |
| `destination` | **（必須）** 、書き出し先の情報：<ul><li>`destination.datasetId`: **（必須）** ：データを書き出すデータセットのID。</li><li>`destination.segmentPerBatch`: *（オプション）* 。指定しなかった場合のデフォルト値はBoolean値です `false`。 の値を指定すると、す `false` べてのセグメントIDが単一のバッチIDにエクスポートされます。 の値は、1つのセグメ `true` ントIDを1つのバッチIDにエクスポートします。 この値を設定すると、バッチエクスポートのパフ `true` ォーマンスに影響を与える場合があります。</li></ul> |
| `schema.name` | **（必須）** 、データを書き出すデータセットに関連付けられているスキーマの名前。 |

>[!NOTE] プロファイルデータのみを書き出し、関連するExperienceEventデータを含めない場合は、「additionalFields」オブジェクトをリクエストから削除します。

**応答**

成功した応答は、リクエストで指定されたプロファイルデータを含むデータセットを返します。

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

リクエスト `destination.segmentPerBatch` に含まれていない場合(存在しない場合はデフォルトで `false`)、またはに設定されていた場合、上記の応答内のオブジェクトは配列を持たず、次に示すように1つのみを含 `false``destination``batches``batchId`みます。 この1つのバッチにはすべてのセグメントIDが含まれ、上の応答にはバッチIDごとに1つのセグメントIDが表示されます。

```json
  "destination": {
    "datasetId": "5cf6bcf79ecc7c14530fe436",
    "segmentPerBatch": false,
    "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
  }
```

## リスト，すべての書き出しジョブ

特定のIMS組織のすべてのエクスポートリストを返すには、エンドポイントに対してGET要求を実行 `export/jobs` します。 リクエストは、以下に示すように、クエリ `limit` パラメー `offset`ターともサポートします。

**API形式**

```http
GET /export/jobs
GET /export/jobs?limit=4
GET /export/jobs?offset=2
```

| プロパティ | 説明 |
| -------- | ----------- |
| `limit` | 返すレコード数を指定します。 |
| `offset` | 指定した数で返される結果のページをオフセットします。 |

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

応答には、IMS組織で作成さ `records` れた書き出しジョブを含むオブジェクトが含まれます。

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

特定の書き出しジョブの詳細を表示したり、処理中の状態を監視したりするには、エンドポイントにGET要求を行い、書き出しジョブのパス `/export/jobs``id` を含めることができます。 フィールドが値「SUCCEEDED」を返すと、エ `status` クスポートジョブが完了します。

**API形式**

```http
GET /export/jobs/{EXPORT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセ `id` スする書き出しジョブの名前。 |

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
| `batchId` | 正常にエクスポートされたバッチから作成され、プロファイルデータを読み取る際に参照目的で使用されるバッチの識別子。 |

## 書き出しジョブのキャンセル

エクスペリエンスプラットフォームでは、既存の書き出しジョブをキャンセルできます。これは、書き出しジョブが完了しなかったか、処理段階で停止した場合など、様々な理由で役立つ場合があります。 書き出しジョブをキャンセルするには、エンドポイントに対してDELETE要求を実行し、キャンセルする書き出しジョ `/export/jobs``id` ブを要求パスに含めることができます。

**API形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセ `id` スする書き出しジョブの名前。 |

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

書き出しが正常に完了すると、データはエクスペリエンスプラットフォームのデータレイク内で使用できます。 その後、 [Data Access API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) （データアクセスAPI）を使用して、エクスポートに関連付けられたを使用し `batchId` てデータにアクセスできます。 エクスポートのサイズに応じて、データはチャンク単位で、バッチは複数のファイルで構成される場合があります。

データアクセスAPIを使用してバッチファイルにアクセスし、ダウンロードする手順については、データアクセスチュートリアルに [従ってください](../../data-access/tutorials/dataset-data.md)。

また、Adobe Experience Platformクエリサービスを使用して、書き出されたリアルタイム顧客プロファイルデータにアクセスすることもできます。 クエリサービスでは、UIまたはRESTful APIを使用して、データレーク内のデータに対するクエリの書き込み、検証および実行を行うことができます。

データのクエリ方法の詳細については、 [クエリサービスのドキュメントを参照してくださ](../../query-service/home.md)い。