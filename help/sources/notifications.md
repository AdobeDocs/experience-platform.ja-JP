---
keywords: Experience Platform;home;popular topics; notifications
description: AdobeI/Oイベントを使用すると、イベントをサブスクライブし、Webフックを使用して、フロー実行の状態に関する通知を受信できます。 これらの通知には、フローの実行の成功に関する情報や、実行の失敗に貢献したエラーに関する情報が含まれます。
solution: Experience Platform
title: フロー実行通知
topic: overview
translation-type: tm+mt
source-git-commit: 690ddbd92f0a2e4e06b988e761dabff399cd2367
workflow-type: tm+mt
source-wordcount: '871'
ht-degree: 4%

---


# フロー実行通知

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNLAdobe Experience Platformフローサービス]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml) は、社内のさまざまなソースから顧客データを収集し、一元管理するために使用 [!DNL Platform]します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

AdobeI/Oイベントを使用すると、イベントをサブスクライブし、Webフックを使用して、フロー実行の状態に関する通知を受信できます。 これらの通知には、フローの実行の成功に関する情報や、実行の失敗に貢献したエラーに関する情報が含まれます。

このドキュメントでは、イベントの登録、Webフックの登録、およびフロー実行のステータスに関する情報を含む通知の受信の手順を説明します。

## はじめに

このドキュメントでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [[!DNL Experience Data Model (XDM) System]](../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
* [[!DNLリアルタイム顧客プロファイル]](../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNLAdobe Experience Platformデータインジェスト]](../ingestion/home.md): [!DNL Data Ingestion] は、これらのソースからデータを [!DNL Platform] 取り込む複数のメソッド、およびダウンストリーム [!DNL Data Lake][!DNL Platform] サービスで使用するために、そのデータがそのソース内でどのように保持されるかを表します。

また、このドキュメントでは、Webフックに関する実際の理解と、あるアプリケーションから別のアプリケーションへのWebフックの接続方法に関する知識も必要です。 Webhookの詳細については、次の [ドキュメント](https://requestbin.com/blog/working-with-webhooks/) を参照してください。

## Webフックの登録

フローの実行状況に関する通知を受け取るには、イベント登録の詳細の一部として一意のWebフックURLを指定して、Webフックを登録する必要があります。 Webフックを [!DNL I/O Events] 購読に接続するには、 [Webフックサービスにアクセスし](https://webhook.site/) 、提供された一意のURLをコピーします。

![WebHook](./images/notifications/webhook-url.png)

## イベントの購読

一意のWebフックURLを取得したら、 [AdobeI/Oイベントに移動し](https://www.adobe.io/apis/experienceplatform/events.html) 、イベントをサブスクライブする開始への [データインジェスト通知](../ingestion/quality/subscribe-events.md) ドキュメントに示されている手順に従います。

>[!IMPORTANT]
>
>購読プロセス中に、イベントプロバイダーとして [!DNL Platform] 通知を選択し、次のイベント購読を選択します。
>
>* **[!UICONTROL Experience Platformソースのフローの実行に成功しました]**
>* **[!UICONTROL Experience Platformソースのフローの実行に失敗しました]**

>
>
Webフックアドレスの入力を求めるプロンプトが表示されたら、以前に取得したWebフックURLを使用します。

## フロー実行通知の受信

Webフックが接続され、イベント購読が完了すると、Webフックダッシュボードを介してフロー実行通知を受信する開始が発生します。

通知は、実行された取り込みジョブの数、ファイルサイズ、エラーなどの情報を返します。 通知は、フロー実行に関連付けられたペイロードもJSON形式で返します。 応答ペイロードは、またはとして分類でき `sources_flow_run_success` ま `sources_flow_run_failure`す。

>[!IMPORTANT]
>
>フロー作成プロセス中に部分的な取り込みが有効な場合、成功した取り込みと失敗した取り込みの両方を含むフローは、フロー作成プロセス中に設定されたエラーのしきい値の割合を下回る場合にのみ、マークされます。 `sources_flow_run_success` 成功したフローの実行にエラーが含まれる場合、これらのエラーは、戻り値のペイロードの一部として含まれます。

### 成功

成功した応答は、特定のフロー実行の特性を定義 `metrics` する一連のセットを返します。この一連のセット `activities` は、データの変換方法を示します。

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
>エラーメッセージの詳細については、 [付録](#errors) を参照してください。

## 次の手順

フローの実行ステータスでリアルタイム通知を受信できるイベントをサブスクライブできるようになりました。 フローの実行とソースの詳細については、 [ソースの概要を参照してください](./home.md)。

## 付録

次の節では、フロー実行通知の操作について詳しく説明します。

### エラーメッセージについて {#errors}

取り込みエラーは、データがソースからコピーされている場合や、コピーされたデータがに処理されている場合に発生する可能性があり [!DNL Platform]ます。 特定のエラーについて詳しくは、次の表を参照してください。

| Error | 説明 |
| ---------- | ----------- |
| `CONNECTOR-1001-500` | ソースからデータをコピー中にエラーが発生しました。 |
| `CONNECTOR-2001-500` | コピーされたデータの処理中にエラーが発生し [!DNL Platform]ました。 このエラーは、解析、検証または変換に関するものです。 |
