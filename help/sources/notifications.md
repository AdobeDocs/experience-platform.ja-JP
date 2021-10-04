---
keywords: Experience Platform；ホーム；人気のあるトピック；通知
description: Web フックを使用して、Adobe I/Oイベントをサブスクライブすることで、ソース接続のフロー実行ステータスに関する通知を受け取ることができます。 これらの通知には、フロー実行の成功または実行の失敗につながったエラーに関する情報が含まれます。
solution: Experience Platform
title: フロー実行通知
topic-legacy: overview
exl-id: 0f1cde97-3030-4b8e-be08-21f64e78b794
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 7%

---

# フロー実行通知

Adobe Experience Platformを使用すると、[!DNL Platform] サービスを使用して、受信データの構造化、ラベル付け、強化を行うことができ、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Flow Service] ](https://www.adobe.io/experience-platform-apis/references/flow-service/) API は、内の様々な異なるソースから顧客データを収集し、一元化するために使用され [!DNL Platform]ます。このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされているすべてのソースから接続できます。

Adobe I/Oイベントを使用すると、イベントをサブスクライブし、Web フックを使用してフロー実行のステータスに関する通知を受け取ることができます。 これらの通知には、フロー実行の成功または実行の失敗につながったエラーに関する情報が含まれます。

このドキュメントでは、イベントのサブスクライブ、Web フックの登録、およびフロー実行の状態に関する情報を含む通知の受信の手順を説明します。

## はじめに

このチュートリアルでは、フロー実行を監視するソース接続が 1 つ以上作成済みであることを前提としています。 まだソース接続を設定していない場合は、まず [ ソースの概要 ](./home.md) にアクセスして、選択したソースを設定してから、このガイドに戻ります。

このドキュメントでは、Web フックに関する十分な知識と、アプリケーション間の Web フックの接続方法についても説明します。 Webhook の概要については、[[!DNL I/O Events] ドキュメント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)を参照してください。

## フロー実行通知用の Webhook の登録

フロー実行通知を受け取るには、Adobe開発者コンソールを使用して、Webhook を [!DNL Experience Platform] 統合に登録する必要があります。

これをおこなう方法の詳細な手順については、[ [!DNL I/O Event]  通知 ](../observability/alerts/subscribe.md) の購読に関するチュートリアルを参照してください。

>[!IMPORTANT]
>
>購読プロセス中に、イベントプロバイダーとして「**[!UICONTROL Platform notifications]**」を選択し、次のイベント購読を選択します。
>
>* **[!UICONTROL Experience Platformソースのフロー実行に成功しました]**
>* **[!UICONTROL Experience Platformソースのフロー実行に失敗しました]**


## フロー実行通知の受信

Webhook を接続し、イベントサブスクリプションが完了したら、Webhook ダッシュボードからフロー実行通知の受信を開始できます。

通知は、実行された取り込みジョブの数、ファイルサイズ、エラーなどの情報を返します。 通知は、フロー実行に関連付けられたペイロードも JSON 形式で返します。 応答ペイロードは、`sources_flow_run_success` または `sources_flow_run_failure` に分類できます。

>[!IMPORTANT]
>
>フロー作成プロセス中に部分取得が有効になっている場合、成功した取り込みと失敗した取り込みの両方を含むフローは、フロー作成プロセス中に設定されたエラーしきい値の割合を下回る場合にのみ `sources_flow_run_success` とマークされます。 成功したフロー実行にエラーが含まれている場合、これらのエラーは、戻りペイロードの一部として引き続き含まれます。

### 成功

正常な応答は、特定のフロー実行の特性を定義する `metrics` と、データの変換方法を示す `activities` のセットを返します。

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
    "imsOrgId": "{IMS_ORG}",
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
              "fileInfo": "https://platform-int.adobe.io/data/foundation/export/batches/01E4TSJNM2H5M74J0XB8MFWDHK/meta?path=input_files"
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
        "imsOrgId": "{IMS_ORG}",
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
| `durationSummary` | フロー実行の開始時刻と終了時刻を定義します。 |
| `sizeSummary` | データのボリュームをバイト単位で定義します。 |
| `recordSummary` | データのレコード数を定義します。 |
| `fileSummary` | データのファイル数を定義します。 |
| `fileInfo` | 正常に取り込まれたファイルの概要を示す URL。 |
| `statusSummary` | フロー実行が成功か失敗かを定義します。 |

### 失敗

次の応答は、失敗したフロー実行の例で、コピーされたデータの処理中にエラーが発生します。 データがソースからコピーされている間も、エラーが発生する可能性があります。 失敗したフロー実行には、実行の失敗に貢献したエラー（エラーや説明など）に関する情報が含まれます。

```json
[
  {
    "messages": [
      {
        "msgType": "eventNotification",
        "version": "1.0",
        "timestamp": 1597434157622,
        "imsOrgId": "{IMS_ORG}",
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
          "imsOrgId": "{IMS_ORG}",
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
>エラーメッセージの詳細については、付録 [ を参照してください。](#errors)

## 次の手順

フローの実行ステータスに関するリアルタイム通知を受信できるイベントをサブスクライブできるようになりました。 フロー実行とソースの詳細については、[ ソースの概要 ](./home.md) を参照してください。

## 付録

次の節では、フロー実行通知の操作に関する追加情報を示します。

### エラーメッセージについて {#errors}

取り込みエラーは、データがソースからコピーされる場合や、コピーされたデータが [!DNL Platform] に処理される場合に発生する可能性があります。 特定のエラーの詳細については、次の表を参照してください。

| エラー | 説明 |
| ---------- | ----------- |
| `CONNECTOR-1001-500` | データをソースからコピー中にエラーが発生しました。 |
| `CONNECTOR-2001-500` | [!DNL Platform] にコピーされたデータの処理中にエラーが発生しました。 このエラーは、解析、検証または変換に関するものです。 |
