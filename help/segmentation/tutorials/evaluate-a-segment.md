---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントの評価
topic: tutorial
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '2841'
ht-degree: 2%

---


# セグメント結果の評価とアクセス

このドキュメントでは、セグメントを評価し、 [Segmentation APIを使用してセグメントの結果にアクセスするためのチュートリアルを提供します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関連する様々なAdobe Experience Platformサービスについて、十分な理解が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

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

## セグメントの評価

セグメント定義を開発、テストおよび保存したら、予定された評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュール評価](#scheduled-evaluation) （「スケジュール済みセグメント化」とも呼ばれます）では、特定の時間にエクスポートジョブを実行するための定期的なスケジュールを作成できます。 [オンデマンド評価では、オーディエンスを即座に作成するセグメントジョブを作成します](#on-demand-evaluation) 。 それぞれの手順を以下に示します。

まだ「Real-time Customer Definition API [」チュートリアルを完了していない場合、または「](./create-a-segment.md) Segment Builderを使用してセグメントプロファイルを作成している場合は、このチュートリアルに進む前に作成してください [](../ui/overview.md)。

## 評価の予定 {#scheduled-evaulation}

定期評価を通じて、IMS組織は定期的なスケジュールを作成し、エクスポートジョブを自動的に実行できます。

>[!NOTE]
>
>スケジュールされた評価は、XDM個別プロファイル用に最大5個のマージポリシーを持つサンドボックスに対して有効にできます。 1つのSandbox環境内にXDM個々のプロファイル用に5つ以上のマージポリシーがある場合、スケジュールされた評価を使用できません。

### スケジュールの作成

エンドポイントにPOSTリクエストを行うことで、スケジュールを作成し、スケジュールをトリガーする特定の時間を含めることがで `/config/schedules` きます。

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
| `type` | **（必須）** 。文字列形式のジョブタイプ。 サポートされているタイプは `batch_segmentation` およびで `export`す。 |
| `properties` | **（必須）** 、集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **(`type`等しい場合に必須)を`batch_segmentation`使用すると**`["*"]` 、すべてのセグメントが確実に含まれます。 |
| `schedule` | **（必須）** 、ジョブスケジュールを含む文字列。 ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間にジョブを2回以上実行するようにスケジュールすることはできません。 次の例(`0 0 1 * * ?`)は、ジョブが毎日1:00:00 UTCでトリガーされることを意味します。 詳しくは、 [cron式形式のドキュメントを参照してください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 |
| `state` | *（オプション）* 、スケジュールの状態を含む文字列。 使用可能な値： `active` と `inactive`。 デフォルト値は `inactive` です。IMS組織で作成できるスケジュールは1つだけです。 スケジュールを更新する手順は、このチュートリアルの後半で説明します。 |

**応答**

正常に応答すると、新たに作成されたスケジュールの詳細が返されます。

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

デフォルトでは、スケジュールは作成時(POST)のリクエスト本文で `state` プロパティがに設定されていない限り、作成時 `active` には非アクティブになります。 エンドポイントにPATCHリクエストを行い、 `state` パスにスケジュールのIDを含めることで、スケジュールを有効にする( `active`toに設定する `/config/schedules` )ことができます。

**API形式**

```http
POST /config/schedules/{SCHEDULE_ID}
```

**リクエスト**

次の要求では、 [JSONパッチの形式設定を使用して](http://jsonpatch.com/) 、スケジュール `state` の更新先を指定し `active`ます。

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

次の要求では、 [JSONパッチの形式設定を使用して](http://jsonpatch.com/) 、スケジュールの [Cron式](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) を更新します。 この例では、スケジュールは10:15:00 UTCでトリガされます。

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

オンデマンド評価では、必要に応じてオーディエンスセグメントを生成するためのセグメントジョブを作成できます。 スケジュールされた評価とは異なり、これはリクエストされた場合にのみ発生し、定期的な評価とは異なります。

### セグメントジョブの作成

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。 セグメント定義と、リアルタイム顧客プロファイルがプロファイルフラグメント間で重なり合う属性をマージする方法を制御するマージポリシーを参照します。 セグメントジョブが正常に完了したら、処理中に発生した可能性のあるエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

リアルタイム顧客プロファイルAPIでエンドポイントにPOSTリクエストを行うことで、新しいセグメントジョブを作成でき `/segment/jobs` ます。

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
| `segmentId` | オーディエンスを作成するセグメント定義の識別子。 ペイロード配列に少なくとも1つのセグメントIDを指定する必要があります。 |

**応答**

正常な応答は、新しく作成されたセグメントジョブの詳細(このセグメントジョブに固有の読み取り専用 `id`の、システム生成の値を含む)を返します。

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
| `id` | 新しいセグメントジョブの識別子。参照用に使用されます。 |
| `status` | セグメントジョブの現在のステータス。 処理が完了するまでは「PROCESSING」となり、その時点で「SUCCEEDEDED」または「FAILED」になります。 |

### セグメントジョブステータスの検索

特定のセグメントジョブ `id` に対してを使用して、ルックアップリクエスト(GET)を実行し、ジョブの現在のステータスを表示できます。

**API形式**

```http
GET /segment/jobs/{SEGMENT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SEGMENT_JOB_ID}` | アクセス `id` するセグメントジョブの名前。 |

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

成功した応答は、セグメント化ジョブの詳細を返し、ジョブの現在のステータスに応じて異なる情報を提供します。 ルックアップリクエストは、が「SUCCEEDED」に `status` 達するまで繰り返すことができ、その際にセグメントをデータセットにエクスポートできます。


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
| `segmentedProfileCounter` | セグメントに合致する、マージされたプロファイルの合計数。 |
| `segmentedProfileByNamespaceCounter` | ID名前空間コードによってセグメントに適合するプロファイルの分類。 ID名前空間コードのリストは、 [ID名前空間の概要に記載されています](../../identity-service/namespaces.md)。 |

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメントに含まれるプロファイルごとに `segmentMembership` マップが更新されます。 `segmentMembership` また、評価済みのオーディエンスセグメントをPlatformに取り込んで保存し、Adobe Audience Managerなどの他のソリューションと統合することもできます。

次の例は、個々のプロファイルレコードの `segmentMembership` 属性がどのように表示されるかを示しています。

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
| `lastQualificationTime` | セグメントのメンバーシップがアサーションされ、プロファイルがセグメントに入るか出るときのタイムスタンプ。 |
| `status` | 現在のリクエストの一部としてのセグメントパーティシペーションのステータス。 次の既知の値のいずれかと等しい必要があります。 <ul><li>`existing`: エンティティが引き続きセグメント内にあります。</li><li>`realized`: エンティティがセグメントを入力しています。</li><li>`exited`: エンティティがセグメントを終了しています。</li></ul> |

## セグメント結果へのアクセス

セグメントジョブの結果は、次の2つの方法のいずれかでアクセスできます。 個々のプロファイルにアクセスしたり、オーディエンス全体をデータセットにエクスポートしたりできます。

次の節で、これらのオプションについて詳しく説明します。

## プロファイルの検索

アクセスする特定のプロファイルがわかっている場合は、Real-time Customer CommenterプロファイルAPIを使用してアクセスできます。 個々のプロファイルにアクセスするための完全な手順は、プロファイルAPIのチュートリアルの「リアルタイム [プロファイルデータにアクセスする](../../profile/api/entities.md) 」で確認できます。

## セグメントのエクスポート {#export}

セグメント化ジョブが正常に完了したら( `status` 属性の値は「SUCCEEDED」です)、オーディエンスをデータセットにエクスポートし、データセットにアクセスして処理できます。

オーディエンスを書き出すには、次の手順が必要です。

- [ターゲットデータセットの作成](#create-a-target-dataset) -オーディエンスメンバーを格納するデータセットを作成します。
- [データセットでのオーディエンスプロファイルの生成](#generate-profiles-for-audience-members) — セグメントジョブの結果に基づいて、XDM Individualプロファイルをデータセットに組み込みます。
- [書き出しの進行状況を監視](#monitor-export-progress) — 書き出し処理の現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) -オーディエンスのメンバを表すXDM個々のプロファイルを取得します。

### ターゲットデータセットの作成

オーディエンスを書き出す場合は、まずターゲットデータセットを作成する必要があります。 データセットを正しく設定して、エクスポートが正常に完了するようにすることが重要です。

重要な考慮事項の1つは、データセットのベースとなるスキーマです(以下のAPIサンプルリクエスト`schemaRef.id` を参照)。 セグメントをエクスポートするには、XDM個別プロファイル和集合スキーマ(`https://ns.adobe.com/xdm/context/profile__union`)に基づくデータセットが必要です。 和集合スキーマは、システム生成の読み取り専用スキーマで、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するスキーマのフィールドを集計します。 和集合表示のスキーマの詳細については、『スキーマレジストリ開発ガイド』の「 [リアルタイム顧客プロファイル」の節を参照してください](../../xdm/api/getting-started.md)。

必要なデータセットを作成する方法は2つあります。

- **APIの使用：** このチュートリアルでは、Catalog APIを使用してXDM Individualプロファイル和集合スキーマを参照するデータセットを作成する方法について説明します。
- **UIの使用：** Adobe Experience Platformのユーザーインターフェイスを使用して和集合スキーマを参照するデータセットを作成するには、 [UIチュートリアルの手順に従って](../ui/overview.md) 、このチュートリアルに戻り、オーディエンスプロファイルの [生成手順に進みます](#generate-xdm-profiles-for-audience-members)。

互換性のあるデータセットが既に存在し、そのIDがわかっている場合は、オーディエンスプロファイルの [生成手順に直接進むことができます](#generate-xdm-profiles-for-audience-members)。

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
    "name": "Segment Export",
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

正常に完了すると、新たに作成されたデータセットの読み取り専用の、システム生成の一意のIDを含む配列が返されます。 オーディエンスメンバーを正常にエクスポートするには、適切に設定されたデータセットIDが必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### オーディエンスメンバのプロファイルの生成

和集合永続性データセットを取得したら、リアルタイム顧客プロファイルAPIでエンドポイントにPOSTリクエストを送信し、エクスポートするセグメントのデータセットIDとセグメント情報を提供することで、オーディエンスをデータセットに永続化するエクスポートジョブを作成できます。 `/export/jobs`

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
    }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `fields` | *（オプション）* 、エクスポートに含めるデータフィールドを、このパラメーターで指定されたデータフィールドのみに制限します。 セグメントの作成時にも同じパラメーターが使用できるので、セグメント内のフィールドが既にフィルターされている可能性があります。 この値を省略すると、書き出されたデータにすべてのフィールドが含まれます |
| `mergePolicy` | *（オプション）* 、エクスポートされたデータを管理するマージポリシーを指定します。 複数のセグメントを書き出す場合は、このパラメーターを含めます。 この値を省略すると、Export Serviceで、セグメントによって提供されるマージポリシーが使用されます。 |
| `mergePolicy.id` | マージポリシーのID |
| `mergePolicy.version` | 使用するマージポリシーの特定のバージョンです。 この値を省略すると、デフォルトで最新バージョンが使用されます。 |
| `filter` | *（オプション）* ：エクスポートの前にセグメントに適用する次のフィルターを1つ以上指定します。 |
| `filter.segments` | *（オプション）* 書き出すセグメントを指定します。 この値を省略すると、すべてのプロファイルのすべてのデータがエクスポートされます。 次のフィールドを含むセグメントオブジェクトの配列を受け入れます。 |
| `filter.segments.segmentId` | **(使用する場合は必須`segments`)** プロファイルのセグメントIDをエクスポートします。 |
| `filter.segments.segmentNs` | *（オプション）* 指定したのセグメント名前空間 `segmentID`。 |
| `filter.segments.status` | *（オプション）* 、のステータスフィルタを提供する文字列の配列 `segmentID`。 デフォルトでは、 `status` は、現在の時間にセグメントに含まれるすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。 使用できる値は次のとおりです。 `"realized"`、 `"existing"`および `"exited"`。 |
| `filter.segmentQualificationTime` | *（オプション）* 、セグメント資格時間に基づいてフィルターします。 開始時間及び/又は終了時間を提供することができる。 |
| `filter.segmentQualificationTime.startTime` | *（オプション）* 特定のステータスのセグメントIDに対するセグメント資格開始時間。 これは指定されていません。セグメントID資格の開始時間に対するフィルターはありません。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.segmentQualificationTime.endTime` | *（オプション）* 特定のステータスのセグメントIDに対するセグメントクオリフィケーションの終了時間。 これは指定されていません。セグメントID資格の終了時間にフィルターは適用されません。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp` | *（オプション）* 、書き出すプロファイルに、このタイムスタンプの後に更新されたアイテムのみが含まれるように制限します。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp` ( **プロファイル**) | マージされた更新されたプロファイルが指定されたタイムスタンプより大きい場合に、マージされたすべてのタイムスタンプを含めます。 オペランドをサポート `greater_than` します。 |
| `filter.fromTimestamp` イベント | このタイムスタンプの後に取り込まれるすべてのイベントは、結果のプロファイル結果に応じて書き出されます。 これは、イベント時間そのものではなく、イベントの取り込み時間です。 |
| `filter.emptyProfiles` | *（オプション）* Boolean。 プロファイルには、プロファイルレコード、ExperienceEventレコード、またはその両方を含めることができます。 プロファイルレコードがなく、ExperienceEventレコードのみのプロファイルは、「emptyProfiles」と呼ばれます。 プロファイルストア内のすべてのプロファイル（「emptyProfiles」も含む）を書き出すには、の値をに設定 `emptyProfiles` し `true`ます。 をに設定 `emptyProfiles``false`すると、ストア内のプロファイルレコードを持つプロファイルのみがエクスポートされます。 デフォルトでは、 `emptyProfiles` 属性が含まれていない場合、プロファイルレコードを含むプロファイルのみがエクスポートされます。 |
| `additionalFields.eventList` | *（オプション）* 次の設定を1つ以上指定して、子オブジェクトまたは関連付けられたオブジェクト用に書き出す時系列イベントフィールドを制御します。 |
| `additionalFields.eventList.fields` | 書き出すフィールドを制御します。 |
| `additionalFields.eventList.filter` | 関連するオブジェクトから含まれる結果を制限する基準を指定します。 エクスポートに必要な最小値（通常は日付）。 |
| `additionalFields.eventList.filter.fromIngestTimestamp` | 指定されたタイムスタンプの後に取り込まれたものに対するフィルター時系列イベント。 これは、イベント時間そのものではなく、イベントの取り込み時間です。 |
| `destination` | **（必須）** 書き出されたデータの保存先情報 |
| `destination.datasetId` | **（必須）** 、データをエクスポートするデータセットのID。 |
| `destination.segmentPerBatch` | *（オプション）* Boolean値。指定しなかった場合のデフォルト値は `false`です。 値を指定すると、すべてのセグメントIDが1つのバッチIDに `false` エクスポートされます。 値として、1つのセグメントIDを1つのバッチIDに `true` エクスポートします。 この値を設定すると、バッチエクスポートのパフォーマンスに影響を与える `true` 場合があります。 |
| `schema.name` | **（必須）** 、データをエクスポートするデータセットに関連付けられているスキーマの名前。 |

**応答**

成功した応答は、セグメントジョブの最後の実行が完了した時点でのプロファイルに該当するジョブが入力されたデータセットを返します。 セグメントジョブの最後の実行が完了した際に、データセットに以前存在したが、そのセグメントに該当しなかったプロファイルはすべて削除されました。

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

### すべての書き出しジョブのリスト

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
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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

### 書き出しの進行状況の監視

書き出しジョブの処理時に、エンドポイントにGETリクエストを行い、書き出しジョブのパスを含めることで、 `/export/jobs` そ `id` のステータスを監視できます。 エクスポートジョブは、 `status` フィールドが値「SUCCEEDEDED」を返すと完了します。

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
| `batchId` | オーディエンスデータを読み取る際に参照用に使用される、成功したエクスポートから作成されたバッチの識別子。 |

## 次の手順

エクスポートが正常に完了すると、Experience Platformのデータレーク内でデータを使用できます。 その後、 [データアクセスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) (Data Access API)を使用して `batchId` 、エクスポートに関連付けられたを使用してデータにアクセスできます。 セグメントのサイズに応じて、データはチャンクになり、バッチは複数のファイルで構成される場合があります。

Data Access APIを使用してバッチファイルにアクセスし、ダウンロードする手順については、「 [データアクセス](../../data-access/tutorials/dataset-data.md)」チュートリアルに従ってください。

Adobe Experience Platformクエリサービスを使用して、正常にエクスポートされたセグメントデータにアクセスすることもできます。 クエリサービスでは、UIまたはRESTful APIを使用して、データレーク内のデータに対してクエリの書き込み、検証、および実行を行うことができます。

オーディエンスデータのクエリ方法の詳細については、 [クエリサービスのドキュメントを参照してください](../../query-service/home.md)。
