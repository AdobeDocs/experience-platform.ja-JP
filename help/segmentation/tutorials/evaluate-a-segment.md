---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントの評価
topic: tutorial
translation-type: tm+mt
source-git-commit: 7f61cee8fb5160d0f393f8392b4ce2462d602981

---


# セグメント結果の評価とアクセス

このドキュメントでは、セグメントを評価し、セグメントAPIを使用してセグメントの結果にアクセスするためのチュートリ [アルを提供しま](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)す。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関わる様々なAdobe Experience Platformサービスについて理解しておく必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

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

> [!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

すべてのPOST、PUTおよびPATCHリクエストには、次の追加のヘッダーが必要です。

- コンテンツタイプ：application/json

## セグメントの評価

セグメント定義を開発、テスト、保存したら、予定された評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュールされた評価](#scheduled-evaluation) （「スケジュールされたセグメント化」とも呼ばれる）では、特定の時間にエクスポートジョブを実行する定期的なスケジュールを作成できます。一方、 [](#on-demand-evaluation) オンデマンド評価では、オーディエンスを即座に構築するセグメントジョブを作成できます。 それぞれの手順を以下に示します。

まだ「リアルタイム顧客プロファイルAPIを使用して [セグメントを作成する」チュートリアルを完了していない場合や、](./create-a-segment.md) Segment Builderを使用してセグメントを作成した場合は [、このチュートリアルに進む前に作成してください](../ui/overview.md)。

## 評価のスケジュール

IMS組織では、定期的な評価を通じて、書き出しジョブを自動的に実行する定期的なスケジュールを作成できます。

> [!NOTE] XDM個々のサンドボックスに対して、最大5個のマージポリシーを持つサンドボックスに対して、スケジュールされた評価を有効にすることができます。プロファイル 1つのサンドボックス環境内にXDM個々のプロファイルに対して5つ以上の結合ポリシーがある場合、スケジュールされた評価を使用できません。

### スケジュールの作成

エンドポイントにPOSTリクエストを行うこ `/config/schedules` とで、スケジュールを作成し、スケジュールをトリガーする特定の時間を含めることができます。

**API形式**

```http
POST /config/schedules
```

**リクエスト**

次のリクエストは、ペイロードで提供された仕様に基づいて新しいスケジュールを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/schedules \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "{SCHEDULE_NAME}",
        "type": "batch_segmentation",
        "properties": {
            "segments": ["*"]
        },
        "schedule": "0 0 1 * * ?",
        "state": "inactive"
        }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | **（必須）** 、スケジュールの名前。 文字列である必要があります。 |
| `type` | **（必須）** 、文字列形式のジョブタイプ。 サポートされているタイプはと `batch_segmentation` です `export`。 |
| `properties` | **（必須）** 、集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **(次に等しい場合に必須`type`)を使用す`batch_segmentation`ると** 、すべてのセグメントが `["*"]` 確実に含まれます。 |
| `schedule` | **（必須）** 、ジョブスケジュールを含む文字列。 ジョブは1日に1回のみ実行するようにスケジュールできます。つまり、24時間に1回以上実行するようにジョブをスケジュールすることはできません。 例(`0 0 1 * * ?`)は、ジョブが毎日1:00:00 UTCにトリガーされることを意味します。 詳しくは、 [cron式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 |
| `state` | *（オプション）* 、スケジュールの状態を含む文字列。 使用可能な値： `active` と `inactive`デフォルト値は `inactive` です。IMS組織で作成できるスケジュールは1つだけです。 スケジュールを更新する手順は、このチュートリアルの後半で説明します。 |

**応答**

正常に応答すると、新しく作成されたスケジュールの詳細が返されます。

```json
{
    "id": "cd585edf-962d-420d-94ad-3be03e619ac2",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "e7e17720-c5bb-11e9-aafb-87c71c35cac8",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "{SCHEDULE_NAME}",
    "state": "inactive",
    "type": "batch_segmentation",
    "schedule": "0 0 1 * * ?",
    "properties": {
        "segments": [
            "*"
        ]
    },
    "createEpoch": 1568267948,
    "updateEpoch": 1568267948
}
```

### スケジュールの有効化

デフォルトでは、作成(POST)要求本文でプロパティがに設定さ `state` れていない限り、スケジュ `active` ールは作成時に非アクティブになります。 エンドポイントにPATCHリクエストを行い、パスにスケジ `state` ュールのIDを含めることで、スケジュールを有効にする( `active`に設定する)こ `/config/schedules` とができます。

**API形式**

```http
POST /config/schedules/{SCHEDULE_ID}
```

**リクエスト**

次のリクエストでは、 [JSONパッチの形式設定を使用し](http://jsonpatch.com/) 、スケジュールの更新先 `state` としてスケジュールを更新しま `active`す。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/schedules/cd585edf-962d-420d-94ad-3be03e619ac2 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        {
          "op": "add",
          "path": "/state",
          "value": "active"
        }
      ]'
```

**応答**

更新が成功すると、空の応答本文とHTTPステータス204（コンテンツなし）が返されます。

同じ操作を使用して、前のリクエストの「値」を「inactive」に置き換えることで、スケジュールを無効にできます。

### スケジュール時刻の更新

スケジュールのタイミングは、エンドポイントにPATCHリクエストを行い、 `/config/schedules` パスにスケジュールのIDを含めることで更新できます。

**API形式**

```http
POST /config/schedules/{SCHEDULE_ID}
```

**リクエスト**

次のリクエストでは、 [JSONパッチの形式設定を使用して](http://jsonpatch.com/) 、スケジュールの [Cron式](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) を更新します。 この例では、スケジュールは10:15:00 UTCでトリガされます。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/schedules/cd585edf-962d-420d-94ad-3be03e619ac2 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        {
          "op": "add",
          "path": "/schedule",
          "value": "0 15 10 * * ?"
        }
      ]'
```

**応答**

更新が成功すると、空の応答本文とHTTPステータス204（コンテンツなし）が返されます。

## オンデマンド評価

オンデマンド評価を使用すると、必要に応じてセグメントセグメントを生成するためのオーディエンスジョブを作成できます。 スケジュールされた評価とは異なり、これはリクエストされた場合にのみ発生し、定期的な評価ではありません。

### セグメントジョブの作成

セグメントジョブは、新しいジョブセグメントを作成する非同期オーディエンスプロセスです。 セグメント定義と、リアルタイム顧客プロファイルが複数のプロファイルフラグメント間で重なり合う属性をマージする方法を制御するマージポリシーを参照します。 セグメントジョブが正常に完了したら、処理中に発生したエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

新しいセグメントジョブを作成するには、Real-time Customer Job APIでエンドポイ `/segment/jobs` ントにPOSTリクエストを作成します。

**API形式**

```http
POST /segment/jobs
```

**リクエスト**

次のリクエストは、ペイロードで提供された2つのセグメント定義に基づいて新しいセグメントジョブを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/segment/jobs \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        {
          "segmentId" : "42f49f2d-edb0-474f-b29d-2799d89cd5a6"
        },
        {
          "segmentId" : "54a20f19-9a0w-293a-9b82-409b1p3v0192"
        }
    ]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentId` | セグメントの作成元のセグメント定義のオーディエンス。 ペイロード配列に少なくとも1つのセグメントIDを指定する必要があります。 |

**応答**

成功した応答は、新しく作成されたセグメントジョブの詳細（このセグメントジョブに固有の読み取り専用の、システム生成の値を含む） `id`を返します。

```json
{
    "profileInstanceId": "ups",
    "computeJobId": 1,
    "id": "b0f99dde-6d3b-4d92-aa92-28072ded71a0",
    "status": "PROCESSING",
    "segments": [
        {
            "segmentId": "42f49f2d-edb0-474f-b29d-2799d89cd5a6",
            "segment": {
                "id": "42f49f2d-edb0-474f-b29d-2799d89cd5a6",
                "version": 1,
                "expression": {
                    "type": "PQL",
                    "format": "pql/text",
                    "value": "homeAddress.country = \"US\""
                },
                "mergePolicy": {
                    "id": "mpid1",
                    "version": 1
                }
            }
        },
        {
            "segmentId": "54a20f19-9a0w-293a-9b82-409b1p3v0192",
            "segment": {
                "id": "54a20f19-9a0w-293a-9b82-409b1p3v0192",
                "version": 1,
                "expression": {
                    "type": "PQL",
                    "format": "pql/text",
                    "value": "homeAddress.country = \"US\""
                },
                "mergePolicy": {
                    "id": "mpid1",
                    "version": 1
                }
            }
        }
    ],
    "updateTime": 1533581808162,
    "imsOrgId": "{IMS_ORG}",
    "creationTime": 1533581808162,
    "_links": {
        "cancel": {
            "href": "/segment/jobs/b0f99dde-6d3b-4d92-aa92-28072ded71a0",
            "method": "DELETE"
        },
        "checkStatus": {
            "href": "/segment/jobs/b0f99dde-6d3b-4d92-aa92-28072ded71a0",
            "method": "GET"
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 参照の目的で使用される、新しいセグメントジョブの識別子。 |
| `status` | セグメントジョブの現在のステータス。 処理が完了するまでは「PROCESSING」となり、その時点で「SUCCEEDED」または「FAILED」になります。 |

### セグメントジョブステータスの検索

特定のセグメントジ `id` ョブに対してを使用して、ジョブの現在のステータスを表示するために、ルックアップリクエスト(GET)を実行できます。

**API形式**

```http
GET /segment/jobs/{SEGMENT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SEGMENT_JOB_ID}` | アクセ `id` スするセグメントジョブの名前。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/segment/jobs/80388706-29fa-40d3-81cf-a297d0224ad9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

成功した応答は、セグメント化ジョブの詳細を返し、ジョブの現在のステータスに応じて異なる情報を提供します。 ルックアップリクエストは、が「SUCCEEDED」に達す `status` るまで繰り返し実行できます。このとき、セグメントをデータセットにエクスポートできます。


```json
{
    "profileInstanceId": "ups",
    "errors": [],
    "computeJobId": 13377,
    "modelName": "_xdm.context.profile",
    "id": "80388706-29fa-40d3-81cf-a297d0224ad9",
    "status": "SUCCEEDED",
    "segments": [
        {
            "segmentId": "b560a09a-de85-4a1c-8477-2f3da1d9e86b",
            "segment": {
                "id": "b560a09a-de85-4a1c-8477-2f3da1d9e86b",
                "version": 1,
                "expression": {
                    "type": "PQL",
                    "format": "pql/json",
                    "value": "homeAddress.country = \"US\""
                },
                "mergePolicy": {
                    "id": "0bf16e61-90e9-4204-b8fa-ad250360957b",
                    "version": 1
                }
            }
        }
    ],
    "requestId": "prgu92v4VNvsGuuXticcsqX96UXGjXtS",
    "computeGatewayJobId": "a7c33b77-3aeb-497f-bc88-807915c57b5f",
    "metrics": {
        "totalTime": {
            "startTimeInMs": 1547063631503,
            "endTimeInMs": 1547063731181,
            "totalTimeInMs": 99678
        },
        "profileSegmentationTime": {
            "startTimeInMs": 1547063669001,
            "endTimeInMs": 1547063720887,
            "totalTimeInMs": 51886
        },
        "segmentedProfileCounter": {
            "ca763983-5572-4ea4-809c-b7dff7e0d79b": 4195,
            "251e3a1f-645c-4444-836b-18e6b668bdf8": 0,
            "3da8bad9-29fb-40e0-b39e-f80322e964de": 0,
            "30230300-ccf1-48ad-8012-c5563a007069": 0
        },
        "segmentedProfileByNamespaceCounter": {
            "ca763983-5572-4ea4-809c-b7dff7e0d79b": {
                "email": 4195
            },
            "251e3a1f-645c-4444-836b-18e6b668bdf8": {},
            "3da8bad9-29fb-40e0-b39e-f80322e964de": {},
            "30230300-ccf1-48ad-8012-c5563a007069": {}
        }     
    },
    "updateTime": 1547063731181,
    "imsOrgId": "{IMS_ORG}",
    "creationTime": 1547063631503,
    "_links": {
        "cancel": {
            "href": "/segment/jobs/80388706-29fa-40d3-81cf-a297d0224ad9",
            "method": "DELETE"
        },
        "checkStatus": {
            "href": "/segment/jobs/80388706-29fa-40d3-81cf-a297d0224ad9",
            "method": "GET"
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentedProfileCounter` | セグメントに適用される、結合されたプロファイルの合計数です。 |
| `segmentedProfileByNamespaceCounter` | IDプロファイルコードによってセグメントに適用される名前空間の分類。 ID名前空間コードのリストは、ID名前空間の概要で確認 [できます](../../identity-service/namespaces.md)。 |

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメ `segmentMembership` ントに含まれる各プロファイルのマップが更新されます。 `segmentMembership` また、評価済みのオーディエンスセグメントをPlatformに取り込んで保存し、Adobe Platform Managerなどの他のソリューションと統合できるようにします。

次の例は、各プロファイルレコー `segmentMembership` ドの属性の外観を示しています。

```json
{
  "segmentMembership": {
    "UPS": {
      "04a81716-43d6-4e7a-a49c-f1d8b3129ba9": {
        "timestamp": "2018-04-26T15:52:25+00:00",
        "status": "existing"
      },
      "53cba6b2-a23b-454a-8069-fc41308f1c0f": {
        "lastQualificationTime": "2018-04-26T15:52:25+00:00",
        "status": "realized"
      }
    },
    "Email": {
      "abcd@adobe.com": {
        "lastQualificationTime": "2017-09-26T15:52:25+00:00",
        "status": "exited"
      }
    }
  }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `lastQualificationTime` | セグメントのメンバーシップのアサーションが行われ、プロファイルがセグメントに入るか出たときのタイムスタンプ。 |
| `status` | 現在のリクエストの一部としてのセグメント貢献度のステータス。 次の既知の値のいずれかと等しい必要があります。 <ul><li>`existing`:エンティティは、引き続きセグメント内に存在します。</li><li>`realized`:エンティティがセグメントを入力しています。</li><li>`exited`:エンティティがセグメントを終了しています。</li></ul> |

## セグメント結果へのアクセス

セグメントジョブの結果には、次の2つの方法のいずれかでアクセスできます。個々のデータにアクセスしたり、プロファイル全体をオーディエンスセットに書き出したりできます。

次の節で、これらのオプションの概要を詳しく説明します。

## 検索プロファイル

アクセスする特定のプロファイルがわかっている場合は、Real-time CustomerプロファイルAPIを使用してアクセスできます。 個々のプロファイルにアクセスするための完全な手順は、プロファイルAPIのチュートリ [アルを使用したリアルタイム顧客プロファイルデータへのアクセスで](../../profile/api/entities.md) 説明します。

## セグメントのエクスポート

セグメント化ジョブが正常に完了したら（属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートし、そこでアクセスし、処理を行うことができます。 `status`

オーディエンスの書き出しには、次の手順が必要です。

- [ターゲットデータセットの作成](#create-a-target-dataset) — データセットを作成して、オーディエンスメンバーを保持します。
- [オーディエンスプロファイルをデータセットに生成する](#generate-profiles-for-audience-members) — セグメントジョブの結果に基づいて、XDM個々のプロファイルをデータセットに埋め込みます。
- [書き出しの進行状況を監視](#monitor-export-progress) — 書き出しの現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) — 結果のXDM個々のプロファイルを取得します。オーディエンスのメンバーです。

### ターゲットデータセットの作成

オーディエンスを書き出す場合は、まずターゲットデータセットを作成する必要があります。 データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の1つは、データセットのベースとなるスキーマ`schemaRef.id` （以下のAPIサンプルリクエスト）です。 セグメントを書き出すには、XDM個々のプロファイル和集合スキーマ(`https://ns.adobe.com/xdm/context/profile__union`)に基づくデータセットが必要です。 和集合スキーマは、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するスキーマのフィールドを集計する、システム生成の読み取り専用のスキーマです。 和集合表示スキーマの詳細については、『スキーマレジストリ開発 [者ガイド』の「リアルタイム顧客プロファイル」の節を参照してください](../../xdm/api/getting-started.md)。

必要なデータセットを作成する方法は2つあります。

- **APIの使用：** このチュートリアルでは、Catalog APIを使用してXDM個々のプロファイル和集合スキーマを参照するデータセットを作成する方法について説明します。
- **UIの使用：** Adobe Experience Platformのユーザーインターフェイスを使用して和集合スキーマを参照するデータセットを作成するには、 [UIチュートリアルの手順に従って](../ui/overview.md) 、このチュートリアルに戻り、オーディエンスプロファイルの生成手順に進 [みます](#generate-xdm-profiles-for-audience-members)。

互換性のあるデータセットが既に存在し、そのIDがわかっている場合は、直接手順に進んでオーディエンスプロファイルを生成で [きます](#generate-xdm-profiles-for-audience-members)。

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
    "name": "Segment Export",
    "schemaRef": {
        "id": "https://ns.adobe.com/xdm/context/profile__union",
        "contentType": "application/vnd.adobe.xed+json;version=1"
    },
    "fileDescription": {
        "persisted": true,
        "containerFormat": "parquet",
        "format": "parquet"
    },
    "aspect": "production"
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | データセットを説明する名前。 |
| `schemaRef.id` | データセットが関連付けられる和集合表示(スキーマ)のID。 |
| `fileDescription.persisted` | に設定した場合、データセットが `true`和集合表示に保持されます。 |

**応答**

成功した場合、新しく作成されたデータセットの読み取り専用の、システム生成の一意のIDを含む配列が返されます。 データセットメンバーを正常にエクスポートするには、適切に設定されたオーディエンスセットIDが必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### プロファイルメンバのオーディエンスの生成

和集合永続的なデータセットを取得したら、Real-time Customer CustomerプロファイルAPIのエンドポイントにPOSTリクエストを送信し、書き出すセグメントのデータセットIDとセグメント情報を提供することで、オーディエンスメンバーをデータセットに永続化する書き出しジョブを作成できます。 `/export/jobs`

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
| `fields` | *（オプション）* 、エクスポートに含めるデータフィールドを、このパラメーターで指定されたデータフィールドのみに制限します。 セグメントの作成時にも同じパラメータが使用できるので、セグメント内のフィールドが既にフィルタされている可能性があります。 この値を省略すると、書き出されたデータにすべてのフィールドが含まれます |
| `mergePolicy` | *（オプション）* 、エクスポートされたデータを管理するマージポリシーを指定します。 複数のセグメントが書き出される場合は、このパラメータを含めます。 この値を省略すると、エクスポートサービスでセグメントが提供するマージポリシーが使用されます。 |
| `mergePolicy.id` | マージポリシーのID |
| `mergePolicy.version` | 使用するマージポリシーの特定のバージョンです。 この値を省略すると、デフォルトで最新バージョンが使用されます。 |
| `filter` | *（オプション）* ：エクスポートの前にセグメントに適用する次のフィルターを1つ以上指定します。 |
| `filter.segments` | *（オプション）* 、書き出すセグメントを指定します。 この値を省略すると、すべてのデータが書き出されるプロファイルになります。 次のフィールドを含むセグメントオブジェクトの配列を受け入れます。 |
| `filter.segments.segmentId` | **(を使用する場合は必須`segments`)** 、エクスポートするプロファイルのセグメントID。 |
| `filter.segments.segmentNs` | *（オプション）* 、指定したのセグメント名前空間 `segmentID`。 |
| `filter.segments.status` | *（オプション）* 、のステータスフィルターを提供する文字列の配 `segmentID`列。 デフォルトでは、 `status` は、現在の時間にセ `["realized", "existing"]` グメントに含まれるすべてのプロファイルを表す値を持ちます。 次の値を指定できます。 `"realized"`、、 `"existing"`および `"exited"`。 |
| `filter.segmentQualificationTime` | *（オプション）* 、セグメントの資格時間に基づいてフィルターします。 開始時間及び/又は終了時間を提供する。 |
| `filter.segmentQualificationTime.startTime` | *（オプション）* 、特定のステータスのセグメントIDのセグメント資格開始時間。 このオプションは指定されていないので、セグメントIDの開始時間に対するフィルターは適用されません。 タイムスタンプは [RFC 3339形式で提供する必要があります](https://tools.ietf.org/html/rfc3339) 。 |
| `filter.segmentQualificationTime.endTime` | *（オプション）* 、特定のステータスのセグメントIDのセグメントクオリフィケーション終了時間。 このオプションは指定されていないので、終了時間にセグメントIDの資格に対するフィルターは適用されません。 タイムスタンプは [RFC 3339形式で提供する必要があります](https://tools.ietf.org/html/rfc3339) 。 |
| `filter.fromIngestTimestamp` | *（オプション）* 、書き出したプロファイルには、このタイムスタンプの後に更新されたアイテムのみが含まれるように制限します。 タイムスタンプは [RFC 3339形式で提供する必要があります](https://tools.ietf.org/html/rfc3339) 。 |
| `filter.fromIngestTimestamp` ( **プロファイル**、指定されている場合) | 結合された更新されたプロファイルが指定されたタイムスタンプより大きい場合、結合されたすべてのタイムスタンプを含めます。 オペランド `greater_than` をサポート |
| `filter.fromTimestamp` イベント | このイベントタイムスタンプの後に取り込まれるすべてのタイムスタンプは、結果のタイムスタンプの結果に対応してプロファイルされます。 これは、イベント時間自体ではなく、インジェスト時間のイベントです。 |
| `filter.emptyProfiles` | *（オプション）* Boolean。 プロファイルには、プロファイルレコード、ExperienceEventレコード、またはその両方を含めることができます。 プロファイルレコードがなく、ExperienceEventレコードのみが含まれるプロファイルは、「emptyProfiles」と呼ばれます。 「emptyProfiles」を含む、プロファイルストア内のすべてのプロファイルを書き出すには、の値をに設定 `emptyProfiles` します `true`。 をに設定 `emptyProfiles` すると、スト `false`ア内のプロファイルレコードを持つプロファイルのみが書き出されます。 デフォルトでは、属性が含ま `emptyProfiles` れていない場合、プロファイルレコードを含むプロファイルのみが書き出されます。 |
| `additionalFields.eventList` | *（オプション）* 次の1つ以上の設定を指定して、子オブジェクトまたは関連オブジェクト用に書き出す時系列イベントフィールドを制御します。 |
| `additionalFields.eventList.fields` | 書き出すフィールドを制御します。 |
| `additionalFields.eventList.filter` | 関連オブジェクトから含まれる結果を制限する基準を指定します。 エクスポートに必要な最小値（通常は日付）。 |
| `additionalFields.eventList.filter.fromIngestTimestamp` | フィルターの時系列イベントは、指定されたタイムスタンプの後に取り込まれたものに対して使用されます。 これは、イベント時間自体ではなく、インジェスト時間のイベントです。 |
| `destination` | **（必須）** 、書き出したデータの保存先情報 |
| `destination.datasetId` | **（必須）** 、データを書き出すデータセットのID。 |
| `destination.segmentPerBatch` | *（オプション）* 。指定しない場合のデフォルト値はBoolean値です `false`。 の値を指定すると、す `false` べてのセグメントIDが単一のバッチIDにエクスポートされます。 の値は、1つのセグメ `true` ントIDを1つのバッチIDにエクスポートします。 この値を設定すると、バッチエクスポートのパフ `true` ォーマンスに影響を与える場合があります。 |
| `schema.name` | **（必須）** 、データを書き出すデータセットに関連付けられているスキーマの名前。 |

**応答**

成功した応答は、セグメントジョブの最後に完了したプロファイルを格納したデータセットを返します。 プロファイルセットに以前存在したが、セグメントジョブの最後の実行中にセグメントに適合しなかった可能性のあるジョブは削除されました。

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

### リスト，すべての書き出しジョブ

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
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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

### 書き出しの進行状況の監視

書き出しジョブの処理時に、エンドポイントにGETリクエストを送信し、書き出しジョブのパスを含め `/export/jobs` ることで、そのステ `id` ータスを監視することができます。 フィールドが値「SUCCEEDED」を返すと、エ `status` クスポートジョブが完了します。

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
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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
| `batchId` | 正常にエクスポートされたバッチから作成され、オーディエンスデータを読み取る際に参照目的で使用されるバッチの識別子。 |

## 次の手順

書き出しが正常に完了すると、データはエクスペリエンスプラットフォームのデータレイク内で使用できます。 その後、 [Data Access API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) （データアクセスAPI）を使用して、エクスポートに関連付けられたを使用し `batchId` てデータにアクセスできます。 セグメントのサイズに応じて、データがチャンク単位で、バッチが複数のファイルで構成される場合があります。

データアクセスAPIを使用してバッチファイルにアクセスし、ダウンロードする手順については、データアクセスチュートリアルに [従ってください](../../data-access/api.md)。

また、Adobe Experience Platform Platform Serviceを使用して、正常に書き出されたセグメントデータにアクセスすることもクエリできます。 クエリサービスでは、UIまたはRESTful APIを使用して、データレーク内のデータに対するクエリの書き込み、検証および実行を行うことができます。

データのクエリ方法の詳細については、 [クエリサービスのドキュメントを参照してくださ](../../query-service/home.md)い。
