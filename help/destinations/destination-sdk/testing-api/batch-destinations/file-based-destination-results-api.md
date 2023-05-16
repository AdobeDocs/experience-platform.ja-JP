---
description: このページでは、/testing/destinationInstance API エンドポイントを使用してテスト結果の完全な詳細を表示する方法について説明します。 この API エンドポイントは、フローサービス API を使用してデータフローを監視する場合と同じ結果を返します。
title: 詳細なアクティベーション結果の表示
exl-id: a7b27beb-825e-47fd-8939-f499c3298f68
source-git-commit: ffd87573b93d642202e51e5299250a05112b6058
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 16%

---

# 詳細なアクティベーション結果の表示 {#view-test-results}

## 概要 {#overview}

このページでは、 `/testing/destinationInstance` API エンドポイントを使用して、ファイルベースの宛先テスト結果の完全な詳細を表示できます。

既に [宛先をテストしました](file-based-destination-testing-api.md) とが有効な API 応答を受け取った場合、宛先は正しく動作しています。

アクティベーションフローの詳細を確認するには、 `results` プロパティを [宛先のテスト](file-based-destination-testing-api.md) エンドポイントの応答（後述）。

>[!NOTE]
>
>この API エンドポイントは、 [フローサービス API](../../../api/update-destination-dataflows.md) データフローを監視する。

## はじめに {#getting-started}

続ける前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 前提条件 {#prerequisites}

使用する前に `/testing/destinationInstance` エンドポイントで、次の条件を満たしていることを確認します。

* Destination SDKを通じて作成された既存のファイルベースの宛先があり、 [宛先カタログ](../../../ui/destinations-workspace.md).
* 宛先 UI に、少なくとも 1 つのアクティベーションフローがExperience Platformされました。
* API リクエストを正常に実行するには、テストする宛先インスタンスに対応する宛先インスタンス ID が必要です。 Platform UI で宛先との接続を参照する際に、API 呼び出しで使用する必要がある宛先インスタンス ID を URL から取得します。

   ![URL から宛先インスタンス ID を取得する方法を示す UI 画像。](../../assets/testing-api/get-destination-instance-id.png)
* 以前 [宛先設定をテストしました](file-based-destination-testing-api.md)を受け取り、有効な API 応答 ( `results` プロパティ。 これを使用します `results` の値を使用して、宛先をさらにテストします。

## 詳細な宛先テスト結果の表示 {#test-activation-results}

一度 [宛先設定を検証しました](file-based-destination-testing-api.md)に値を入力すると、詳細なアクティベーション結果を表示できます。それには、 `authoring/testing/destinationInstance/` エンドポイントを作成し、テストする宛先の宛先インスタンス ID と、アクティブ化されたセグメントのフロー実行 ID を指定します。

使用する必要のある完全な API URL は、 `results` プロパティが [宛先テスト呼び出しの応答](file-based-destination-testing-api.md).

**API 形式**

```http
GET /authoring/testing/destinationInstance/{DESTINATION_INSTANCE_ID}/results?flowRunIds=id1,id2
```

| パスパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | サンプルプロファイルを生成する宛先インスタンスの ID。 詳しくは、 [前提条件](#prerequisites) の節を参照してください。 |

| クエリ文字列パラメーター | 説明 |
| -------- | ----------- |
| `flowRunIds` | アクティブ化されたセグメントに対応するフロー実行 ID。 フロー実行 ID は、 `results` プロパティが [宛先テスト呼び出しの応答](file-based-destination-testing-api.md). |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/core/activation/authoring/testing/destinationInstance/fd3449fb-b929-45c8-9f3d-06b9d6aac328/results?flowRunIds=30d34875-e7ba-4520-ab6e-5705e01dfb16,86c00ad7-443c-459a-855d-0e8cbee43c4f,12305c58-42a9-4230-8fad-1661ee49cb70' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、アクティベーションフローの完全な詳細が含まれます。 同じ応答を取得するには、 [フローサービス API](../../../api/update-destination-dataflows.md) データフローを監視する。

```json
{
   "items":[
      {
         "id":"18efd5d2-40ae-4f5c-afd1-37a39a45183a",
         "flowId":"a02071ad-f3a4-496c-a2b1-468812301d5d",
         "flowSpec":{
            "id":"25473b67-0801-418a-ab49-ed74ebf88137",
            "version":"1.0"
         },
         "metrics":{
            "durationSummary":{
               "startedAtUTC":1646652235124,
               "completedAtUTC":1646652270439
            },
            "latencySummary":null,
            "sizeSummary":{
               "inputBytes":122,
               "outputBytes":122
            },
            "recordSummary":{
               "inputRecordCount":1,
               "outputRecordCount":1,
               "createdRecordCount":1,
               "skippedRecordCount":0,
               "sourceSummaries":[
                  {
                     "id":"76e4b969-9700-4557-8330-0a8390afbdde",
                     "entitySummaries":[
                        {
                           "inputRecordCount":1,
                           "skippedRecordCount":0,
                           "id":"segment:4326c566-f81c-4ab0-8a80-9e741a5d0b1f"
                        }
                     ]
                  }
               ],
               "targetSummaries":[
                  {
                     "id":"b43607b6-0dca-43b3-a0bc-ecdea4fa6aa9",
                     "entitySummaries":[
                        {
                           "outputRecordCount":1,
                           "createdRecordCount":1,
                           "id":"segment:4326c566-f81c-4ab0-8a80-9e741a5d0b1f"
                        }
                     ]
                  }
               ]
            },
            "fileSummary":{
               "inputFileCount":1,
               "outputFileCount":1
            },
            "statusSummary":{
               "status":"success"
            }
         },
         "activities":[
            {
               "id":"c4f238e3-7334-4933-8b56-64d7ea43ea54",
               "name":"Activation Batch XdmProcessor Activity",
               "updatedAtUTC":0,
               "durationSummary":{
                  "startedAtUTC":1646652235124,
                  "completedAtUTC":1646652255157
               },
               "latencySummary":{
                  
               },
               "sizeSummary":{
                  "inputBytes":122,
                  "outputBytes":122
               },
               "recordSummary":{
                  "inputRecordCount":1,
                  "outputRecordCount":1,
                  "createdRecordCount":1,
                  "skippedRecordCount":0
               },
               "fileSummary":{
                  "inputFileCount":1,
                  "outputFileCount":1
               },
               "statusSummary":{
                  "status":"success",
                  "extensions":{
                     "incremental.batchId":"",
                     "snapshot.batchId":"",
                     "snapshot.datasetId":"",
                     "incremental.datasetId":""
                  }
               },
               "sourceInfo":null,
               "targetInfo":null
            },
            {
               "id":"51d82b36-6b8f-11eb-9439-0242ac130002",
               "name":"Activation Batch Publisher Activity",
               "updatedAtUTC":0,
               "durationSummary":{
                  "startedAtUTC":1646652270326,
                  "completedAtUTC":1646652270439
               },
               "latencySummary":{
                  
               },
               "sizeSummary":{
                  "outputBytes":122
               },
               "recordSummary":{
                  "inputRecordCount":1,
                  "outputRecordCount":1,
                  "createdRecordCount":1,
                  "skippedRecordCount":0
               },
               "fileSummary":{
                  "outputFileCount":1
               },
               "statusSummary":{
                  "status":"success",
                  "extensions":{
                     
                  }
               },
               "sourceInfo":null,
               "targetInfo":null
            }
         ],
         "predecessors":null
      }
   ],
   "_links":{
      
   }
}
```

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読んだ後、ファイルベースの宛先設定をテストする方法と、アクティベーション結果の詳細を確認する方法がわかりました。

公開先を作成する場合、 [宛先設定を送信](../../guides/submit-destination.md) をAdobeに追加します。
