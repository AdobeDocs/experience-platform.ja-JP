---
keywords: Experience Platform；ホーム；人気の高いトピック；通知
description: Adobe I/Oイベントをサブスクライブすると、Webフックを使用して、ソース接続のフロー実行ステータスに関する通知を受信できます。 これらの通知には、フローの実行の成功に関する情報や、実行の失敗に貢献したエラーに関する情報が含まれます。
solution: Experience Platform
title: フロー実行通知
topic-legacy: overview
exl-id: 0f1cde97-3030-4b8e-be08-21f64e78b794
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 5%

---

# フロー実行通知

Adobe Experience Platformは、[!DNL Platform]サービスを使用して、外部ソースからデータを取り込むと同時に、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Adobe Experience Platform Flow Service]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml) は、社内のさまざまなソースから顧客データを収集し、一元化するために使用 [!DNL Platform]します。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

Adobe I/Oイベントを使用すると、イベントをサブスクライブし、Webフックを使用して、フロー実行の状態に関する通知を受信できます。 これらの通知には、フローの実行の成功に関する情報や、実行の失敗に貢献したエラーに関する情報が含まれます。

このドキュメントでは、イベントの登録、Webフックの登録、およびフロー実行のステータスに関する情報を含む通知の受信の手順を説明します。

## はじめに

このチュートリアルでは、フローを監視するソース接続が既に1つ以上作成済みであることを前提としています。 ソース接続をまだ設定していない場合は、[ソースの概要](./home.md)にアクセスし、選択したソースを設定してから、このガイドに戻ってください。

また、このドキュメントでは、Webフックに関する実際の理解と、あるアプリケーションから別のアプリケーションへのWebフックの接続方法に関する知識も必要です。 Webhookの紹介については、[[!DNL I/O Events] ドキュメント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)を参照してください。

## Webフックのフロー実行通知の登録

フロー実行通知を受け取るには、Adobeデベロッパーコンソールを使用してWebフックを[!DNL Experience Platform]統合に登録する必要があります。

これを行う方法の詳細な手順については、[ [!DNL I/O Event] 通知](../observability/notifications/subscribe.md)のサブスクライブのチュートリアルに従ってください。

>[!IMPORTANT]
>
>購読プロセス中に、イベントプロバイダーとして&#x200B;**[!UICONTROL Platform notifications]**&#x200B;を選択し、次のイベント購読を選択します。
>
>* **[!UICONTROL Experience Platformソースのフローの実行に成功しました]**
>* **[!UICONTROL Experience Platformソースのフローの実行に失敗しました]**


## フロー実行通知の受信

Webフックが接続され、イベント購読が完了すると、Webフックダッシュボードを介してフロー実行通知を受信する開始が発生します。

通知は、実行された取り込みジョブの数、ファイルサイズ、エラーなどの情報を返します。 通知は、フロー実行に関連付けられたペイロードもJSON形式で返します。 応答ペイロードは、`sources_flow_run_success`または`sources_flow_run_failure`に分類できます。

>[!IMPORTANT]
>
>フローの作成プロセス中に部分的な取り込みが有効になっている場合、成功した取り込みと失敗した取り込みの両方を含むフローは、エラー数がフローの作成プロセス中に設定されたエラーしきい値の割合を下回る場合にのみ`sources_flow_run_success`としてマークされます。 成功したフローの実行にエラーが含まれる場合、これらのエラーは、戻り値のペイロードの一部として含まれます。

### 成功

成功した応答は、特定のフロー実行の特性を定義する`metrics`と、データの変換方法を示す`activities`のセットを返します。

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
| `activities` | データ変換のために実行される様々な手順とアクティビティを定義します。 |
| `durationSummary` | フロー実行の開始と終了時間を定義します。 |
| `sizeSummary` | データのボリュームをバイト単位で定義します。 |
| `recordSummary` | データのレコード数を定義します。 |
| `fileSummary` | データのファイル数を定義します。 |
| `fileInfo` | 正常に取り込まれたファイルの概要を示すURLです。 |
| `statusSummary` | フローの実行が成功か失敗かを定義します。 |

### 失敗

次の応答は、失敗したフローの実行の例で、コピーされたデータの処理時にエラーが発生します。 ソースからデータをコピー中にエラーが発生する場合もあります。 失敗したフロー実行には、実行の失敗に貢献したエラー（エラーや説明など）に関する情報が含まれます。

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
| `fileInfo` | 正常に取り込まれたファイルと取り込まれなかったファイルの概要を示すURLです。 |

>[!NOTE]
>
>エラーメッセージの詳細については、[付録](#errors)を参照してください。

## 次の手順

フローの実行ステータスでリアルタイム通知を受信できるイベントをサブスクライブできるようになりました。 フローの実行とソースについて詳しくは、[ソースの概要](./home.md)を参照してください。

## 付録

次の節では、フロー実行通知の操作について詳しく説明します。

### エラーメッセージについて{#errors}

インジェストエラーは、データがソースからコピーされている場合や、コピーされたデータが[!DNL Platform]に処理されている場合に発生する可能性があります。 特定のエラーについて詳しくは、次の表を参照してください。

| エラー | 説明 |
| ---------- | ----------- |
| `CONNECTOR-1001-500` | ソースからデータをコピー中にエラーが発生しました。 |
| `CONNECTOR-2001-500` | コピーされたデータを[!DNL Platform]に処理中にエラーが発生しました。 このエラーは、解析、検証または変換に関するものです。 |
