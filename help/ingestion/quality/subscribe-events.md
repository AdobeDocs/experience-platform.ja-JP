---
keywords: Experience Platform；ホーム；人気のあるトピック；データ取得通知；通知；購読イベント；データ取得ステータスイベント；ステータスイベント；購読；ステータス通知；
solution: Experience Platform
title: データ取得通知
topic-legacy: overview
description: インジェストプロセスの監視を支援するために、Adobe Experience Platformでは、プロセスの各手順で公開された一連のイベントをサブスクライブし、取り込んだデータのステータスと考えられるエラーを通知できます。
exl-id: fd34e1ab-f6f6-44f0-88ee-7020e9322c39
source-git-commit: a455134a45137b171636d6525ce9124bc95f4335
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 29%

---

# データ取得通知

Adobe Experience Platform でデータを取得するプロセスは、複数の手順で構成されます。[!DNL Platform]に取り込む必要のあるデータファイルを特定したら、取り込みプロセスが開始され、データが正常に取り込まれるか、取り込まれないまで各手順が連続して実行されます。 取得処理は、[Adobe データ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) を使用するか、Experience Platform のユーザーインターフェイスを使用して開始することができます。[!DNL Experience Platform]

[!DNL Platform]に読み込まれるデータは、宛先、[!DNL Data Lake]または[!DNL Real-time Customer Profile]データストアに到達するために、複数の手順を実行する必要があります。 各手順では、データの処理やデータの検証が行われ、データが次の手順に渡される前にデータが保存されます。取得されるデータの量によっては、この処理に時間がかかる場合があり、検証、セマンティクスまたは処理エラーが原因でプロセスが失敗する可能性が常にあります。失敗した場合は、データの問題を修正し、修正したデータファイルを使用して取得プロセス全体を再開する必要があります。

取り込みプロセスの監視を支援するために、[!DNL Experience Platform]は、プロセスの各手順で公開された一連のイベントをサブスクライブし、取り込んだデータのステータスと考えられるエラーを通知できます。

## データ取得通知用のWebhookの登録

データ取得通知を受け取るには、[Adobe開発者コンソール](https://www.adobe.com/go/devs_console_ui)を使用して、WebhookをExperience Platform統合に登録する必要があります。

これをおこなう方法の詳細な手順については、[ [!DNL Adobe I/O Event] 通知](../../observability/alerts/subscribe.md)の購読に関するチュートリアルを参照してください。

>[!IMPORTANT]
>
>サブスクリプションプロセス中に、イベントプロバイダーとして「**[!UICONTROL Platform notifications]**」を選択し、プロンプトが表示されたら「**[!UICONTROL Data ingestion notification]**」イベントサブスクリプションを選択します。

## データ取得通知の受信

Webhookの登録が完了し、新しいデータが取り込まれたら、イベント通知の受信を開始できます。 これらのイベントは、Webhook自体を使用するか、Adobe開発者コンソールでプロジェクトのイベント登録の概要の「**[!UICONTROL デバッグトレース]**」タブを選択して表示できます。

次のJSONは、バッチ取得イベントが失敗した場合にWebhookに送信される通知ペイロードの例です。

```json
{
  "event_id": "93a5b11a-b0e6-4b29-ad82-81b1499cb4f2",
  "event": {
    "xdm:ingestionId": "01EGK8H8HF9JGFKNDCABHGA24G",
    "xdm:customerIngestionId": "01EGK8H8HF9JGFKNDCABHGA24G",
    "xdm:imsOrg": "{IMS_ORG}",
    "xdm:completed": 1598374341560,
    "xdm:datasetId": "5e55b556c2ae4418a8446037",
    "xdm:eventCode": "ing_load_failure",
    "xdm:sandboxName": "prod",
    "sentTime": "1598374341595",
    "processStartTime": 1598374342614,
    "transformedTime": 1598374342621,
    "header": {
      "_adobeio": {
        "imsOrgId": "{IMS_ORG}",
        "providerMetadata": "aep_observability_catalog_events",
        "eventCode": "platform_event"
      }
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `event_id` | 通知用の、システムで生成された一意のID。 |
| `event` | 通知をトリガーしたイベントの詳細を含むオブジェクト。 |
| `event.xdm:datasetId` | 取得イベントが適用されるデータセットのID。 |
| `event.xdm:eventCode` | データセットに対してトリガーされたイベントのタイプを示すステータスコード。 特定の値とその定義については、[付録](#event-codes)を参照してください。 |

イベント通知の完全なスキーマを表示するには、[パブリックGitHubリポジトリ](https://github.com/adobe/xdm/blob/master/schemas/notifications/ingestion.schema.json)を参照してください。

## 次の手順

プロジェクトに[!DNL Platform]通知を登録すると、[!UICONTROL プロジェクトの概要]で受信したイベントを確認できます。 イベントのトレース方法の詳細については、[Adobe I/Oイベント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md)のトレースガイドを参照してください。

## 付録

次の節では、データ取得通知ペイロードの解釈に関する追加情報について説明します。

### 使用可能なステータス通知イベント {#event-codes}

次の表に、サブスクライブできる、使用可能なデータ取得ステータス通知を示します。

| イベントコード | プラットフォームサービス | ステータス | イベントの説明 |
| --- | ---------------- | ------ | ----------------- |
| `ing_load_success` | [!DNL Data Ingestion] | 成功 | バッチが[!DNL Data Lake]内のデータセットに正常に取り込まれました。 |
| `ing_load_failure` | [!DNL Data Ingestion] | 失敗 | バッチを[!DNL Data Lake]内のデータセットに取り込めませんでした。 |
| `ps_load_success` | [!DNL Real-time Customer Profile] | 成功 | バッチが[!DNL Profile]データストアに正常に取り込まれました。 |
| `ps_load_failure` | [!DNL Real-time Customer Profile] | 失敗 | バッチを[!DNL Profile]データストアに取り込めませんでした。 |
| `ig_load_success` | [!DNL Identity Service] | 成功 | データはIDグラフに正常に読み込まれました。 |
| `ig_load_failure` | [!DNL Identity Service] | 失敗 | データをIDグラフに読み込めませんでした。 |

>[!NOTE]
>
> すべてのデータ取得通知に対して 1 つのイベントトピックのみが提供されます。異なるステータスを区別するために、イベントコードを使用できます。
