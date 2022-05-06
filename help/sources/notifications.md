---
keywords: Experience Platform；ホーム；人気の高いトピック；通知
description: Adobe I/Oイベントを購読すると、Web フックを使用して、ソース接続のフロー実行ステータスに関する通知を受け取ることができます。 これらの通知には、フロー実行の成功または実行の失敗に貢献したエラーに関する情報が含まれます。
solution: Experience Platform
title: フロー実行通知
topic-legacy: overview
exl-id: 0f1cde97-3030-4b8e-be08-21f64e78b794
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 17%

---

# フロー実行通知

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) は、 [!DNL Platform]. このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされるすべてのソースから接続できます。

Adobe I/Oイベントを使用すると、イベントにサブスクライブし、Web フックを使用して、フロー実行のステータスに関する通知を受け取ることができます。 これらの通知には、フロー実行の成功または実行の失敗に貢献したエラーに関する情報が含まれます。

このドキュメントでは、イベントのサブスクライブ、Web フックの登録、およびフロー実行の状態に関する情報を含む通知の受信の手順を説明します。

## はじめに

このチュートリアルでは、フロー実行を監視するソース接続が既に 1 つ以上作成されていることを前提としています。 ソース接続をまだ設定していない場合は、まず [ソースの概要](./home.md) をクリックして、選択したソースを設定してから、このガイドに戻ります。

このドキュメントでは、Web フックに関する十分な知識と、Webhook をアプリケーション間で接続する方法についても説明します。 Webhook の概要については、[[!DNL I/O Events] ドキュメント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)を参照してください。

## フロー実行通知用の Webhook の登録

フロー実行通知を受け取るには、 Adobe Developerコンソールを使用して、 [!DNL Experience Platform] 統合とも呼ばれます。

次のチュートリアルに従います。 [[!DNL I/O Event] 通知の購読](../observability/alerts/subscribe.md) を参照してください。

>[!IMPORTANT]
>
>購読プロセス中に、必ず **[!UICONTROL プラットフォーム通知]** をイベントプロバイダーとして選択し、次のイベント購読を選択します。
>
>* **[!UICONTROL Experience Platformソースのフロー実行に成功しました]**
>* **[!UICONTROL Experience Platformソースのフロー実行に失敗しました]**


## フロー実行通知を受信

Webhook が接続され、イベント購読が完了したら、Webhook ダッシュボードからフロー実行通知の受信を開始できます。

通知は、実行された取り込みジョブの数、ファイルサイズ、エラーなどの情報を返します。 通知は、フロー実行に関連付けられたペイロードも JSON 形式で返します。 応答ペイロードは、 `sources_flow_run_success` または `sources_flow_run_failure`.

>[!IMPORTANT]
>
>フロー作成プロセス中に部分取り込みが有効になっている場合、成功した取り込みと失敗した取り込みの両方を含むフローは、 `sources_flow_run_success` エラー数がフロー作成プロセス中に設定されたエラーしきい値の割合を下回る場合のみ。 成功したフロー実行にエラーが含まれる場合、これらのエラーは、戻りペイロードの一部として引き続き含まれます。

### 成功

正常な応答は、 `metrics` 特定のフロー実行の特性を定義し、 `activities` データを変換する方法の概要を示します。

```json
{
  "event_id": "aec55616-1715-487f-8044-ba648cc8ffee",
  "event": {
    "createdAt": 1597213529158,
    "updatedAt": 1597213530760,
    "createdBy": "{CREATED_BY}",
    "updatedBy": "{UPDATED_BY}",
    "createdClient": "{CREATED_CLIENT}",
    "updatedClient": "{UPDATED_CLIENT}",
    "sandboxId": "7127a4f0-def8-11e9-83ce-e79494b1c2a5",
    "sandboxName": "prod",
    "imsOrgId": "{ORG_ID}",
    "id": "933cf9f4-cf01-4d75-bcf9-f4cf010d750a",
    "flowId": "1c6f1047-dcaf-48fe-af10-47dcaf08feaf",
    "providerRefId": "test1234",
    "etag": "\"5100ec97-0000-0200-0000-5f338b5b0000\"",
    "metrics": {
      "durationSummary": {
        "startedAtUTC": 1590512053,
        "completedAtUTC": 1590512053
      },
      "sizeSummary": {
        "inputBytes": 2048,
        "outputBytes": 1024
      },
      "recordSummary": {
        "inputRecordCount": 100,
        "outputRecordCount": 70
      },
      "fileSummary": {
        "inputFileCount": 10,
        "outputFileCount": 10
      },
      "statusSummary": {
        "status": "success"
      }
    },
    "activities": [
      {
        "id": "copyActivity",
        "updatedAtUTC": 87473822,
        "durationSummary": {
          "startedAtUTC": 1590512053,
          "completedAtUTC": 1590512053
        },
        "sizeSummary": {
          "inputBytes": 2048,
          "outputBytes": 1098
        },
        "recordSummary": {
          "inputRecordCount": 100,
          "outputRecordCount": 100
        },
        "fileSummary": {
          "inputFileCount": 10,
          "outputFileCount": 10
        },
        "statusSummary": {
          "status": "success",
          "extensions": {
            "adf/pipeline/id": "abcd",
            "adf/run/id": "1234"
          }
        },
        "sourceInfo": [
          {
            "id": "sourceConnectionId1",
            "type": "SourceConnection",
            "reference": {
              "type": "AdfRunId"
            }
          }
        ]
      },
      {
        "id": "promotionActivity",
        "updatedAtUTC": 87473822,
        "durationSummary": {
          "completedAtUTC": 1590512053
        },
        "sizeSummary": {
          "inputBytes": 1098,
          "outputBytes": 1024
        },
        "recordSummary": {},
        "fileSummary": {
          "inputFileCount": 10,
          "outputFileCount": 10,
          "extensions": {
            "manifest": {
              "fileInfo": "https://platform.adobe.io/data/foundation/export/batches/01E4TSJNM2H5M74J0XB8MFWDHK/meta?path=input_files"
            }
          }
        },
        "statusSummary": {
          "status": "success",
          "extensions": {
            "batchId": "b1",
            "acp_request_id": "1234"
          }
        },
        "targetInfo": [
          {
            "id": "targetConnectionId1",
            "type": "TargetConnection",
            "reference": {
              "type": "batch"
            }
          }
        ]
      }
    ],
    "slaCreatedAt": 1597213531124,
    "processStartTime": 1597213531213,
    "header": {
      "_adobeio": {
        "imsOrgId": "{ORG_ID}",
        "providerMetadata": "platform_notifications",
        "eventCode": "sources_flow_run_success"
      }
    },
    "transformedTime": 1597213531214
  }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `metrics` | フロー実行のデータの特性を定義します。 |
| `activities` | データを変換するために実行される様々な手順とアクティビティを定義します。 |
| `durationSummary` | フロー実行の開始と終了の時間を定義します。 |
| `sizeSummary` | データのボリュームをバイト単位で定義します。 |
| `recordSummary` | データのレコード数を定義します。 |
| `fileSummary` | データのファイル数を定義します。 |
| `fileInfo` | 正常に取り込まれたファイルの概要を示す URL。 |
| `statusSummary` | フロー実行が成功か失敗かを定義します。 |

### 失敗

次の応答は、失敗したフロー実行の例で、コピーされたデータが処理される際にエラーが発生します。 また、データがソースからコピーされている間にエラーが発生する場合もあります。 失敗したフロー実行には、実行の失敗に貢献したエラーに関する情報（エラーや説明など）が含まれます。

```json
[
  {
    "messages": [
      {
        "msgType": "eventNotification",
        "version": "1.0",
        "timestamp": 1597434157622,
        "imsOrgId": "{ORG_ID}",
        "schema": {
          "name": "run-notification",
          "version": "1.0"
        },
        "provider": "FlowService",
        "_eventNotificationMeta": {
          "category": "Platform Notifications",
          "type": "sources_flow_run_failed"
        },
        "value": {
          "createdAt": 1597434147259,
          "updatedAt": 1597434157567,
          "createdBy": "{CREATED_BY}",
          "updatedBy": "{UPDATED_BY}",
          "createdClient": "{CREATED_CLIENT}",
          "updatedClient": "{UPDATED_CLIENT}",
          "sandboxId": "e49ebb00-d0fa-11e9-b164-ed6a398c8b35",
          "sandboxName": "prod",
          "imsOrgId": "{ORG_ID}",
          "id": "d9024c32-2174-4271-824c-322174627101",
          "flowId": "cf4fce79-8822-456d-8fce-798822556dc6",
          "etag": "\"0c003dbf-0000-0200-0000-5f36e92d0000\"",
          "metrics": {
            "durationSummary": {
              "startedAtUTC": 1597434147190
            },
            "sizeSummary": {
              "inputBytes": -1
            },
            "fileSummary": {
              "inputFileCount": -1
            },
            "statusSummary": {
              "status": "failed",
              "errors": [
                {
                  "code": "CONNECTOR-2001-500",
                  "message": "Error in processing (parsing, validation or transformation) the copied data."
                }
              ]
            }
          },
          "activities": [
            {
              "id": "promotionActivity",
              "updatedAtUTC": 1597434157529,
              "durationSummary": {
                "startedAtUTC": 1597434147190,
                "completedAtUTC": 1597434157212
              },
              "sizeSummary": {
                "inputBytes": -1
              },
              "recordSummary": {},
              "fileSummary": {
                "inputFileCount": -1,
                "extensions": {
                  "manifest": {
                    "fileInfo": "https://platform-stage.adobe.io/data/foundation/export/batches/6f6a900f-e40d-4f0e-9bb9-b614436c3465/meta?path=input_files"
                  }
                }
              },
              "statusSummary": {
                "status": "failed",
                "errors": [
                  {
                    "code": "CONNECTOR-2001-500",
                    "message": "Error in processing (parsing, validation or transformation) the copied data."
                  }
                ],
                "extensions": {
                  "errors": [
                    {
                      "code": "133",
                      "message": "We are unable to locate any files uploaded for this batch. Please upload files to ingest."
                    }
                  ]
                }
              },
              "targetInfo": [
                {
                  "id": "e88737aa-27b8-4795-8737-aa27b8f7959e",
                  "type": "TargetConnection",
                  "reference": {
                    "type": "Batch",
                    "ids": [
                      "6f6a900f-e40d-4f0e-9bb9-b614436c3465"
                    ]
                  }
                }
              ]
            }
          ]
        }
      }
    ]
  }
]
```

| プロパティ | 説明 |
| ---------- | ----------- |
| `fileInfo` | 正常に取り込まれたファイルと正常に取り込まれなかったファイルの概要を示す URL。 |

>[!NOTE]
>
>詳しくは、 [付録](#errors) を参照してください。

## 次の手順

フローの実行ステータスに関するリアルタイム通知を受け取れるイベントをサブスクライブできるようになりました。 フロー実行とソースの詳細については、 [ソースの概要](./home.md).

## 付録

次の節では、フロー実行通知の操作に関する追加情報を示します。

### エラーメッセージについて {#errors}

取り込みエラーは、データがソースからコピーされる場合、またはコピーされたデータがに処理される場合に発生する可能性があります [!DNL Platform]. 特定のエラーの詳細については、以下の表を参照してください。

| エラー | 説明 |
| ---------- | ----------- |
| `CONNECTOR-1001-500` | データをソースからコピー中にエラーが発生しました。 |
| `CONNECTOR-2001-500` | にコピーされたデータの処理中にエラーが発生しました [!DNL Platform]. このエラーは、解析、検証または変換に関するものである可能性があります。 |
