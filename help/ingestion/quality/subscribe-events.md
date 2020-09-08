---
keywords: Experience Platform;home;popular topics;data ingestion notifications;notifications;subscribe events;data ingestion status events;status events;subscribe;status notifications;
solution: Experience Platform
title: データ取得イベントへのサブスクライブ
topic: overview
translation-type: tm+mt
source-git-commit: 80a1694f11cd2f38347989731ab7c56c2c198090
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 32%

---


# データ取得通知

Adobe Experience Platform でデータを取得するプロセスは、複数の手順で構成されます。Once you identify data files that need to be ingested into [!DNL Platform], the ingestion process begins and each step occurs consecutively until the data is either successfully ingested or fails. 取得処理は、[Adobe データ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) を使用するか、Experience Platform のユーザーインターフェイスを使用して開始することができます。[!DNL Experience Platform]

Data loaded into [!DNL Platform] must go through multiple steps in order to reach its destination, the [!DNL Data Lake] or the [!DNL Real-time Customer Profile] data store. 各手順では、データの処理やデータの検証が行われ、データが次の手順に渡される前にデータが保存されます。取得されるデータの量によっては、この処理に時間がかかる場合があり、検証、セマンティクスまたは処理エラーが原因でプロセスが失敗する可能性が常にあります。失敗した場合は、データの問題を修正し、修正したデータファイルを使用して取得プロセス全体を再開する必要があります。

To assist in monitoring the ingestion process, [!DNL Experience Platform] makes it possible to subscribe to a set of events that are published by each step of the process, notifying you to the status of the ingested data and any possible failures.

## データインジェスト通知用のWebフックの登録

データインジェスト通知を受け取るには、 [Adobeデベロッパーコンソール](https://www.adobe.com/go/devs_console_ui) (DDC)を使用してWebフックをExperience Platform統合に登録する必要があります。

これを行う方法について詳しくは、 [購読に関するチュートリアルに従って [!DNL Adobe I/O Event] 通知を行います](../../observability/notifications/subscribe.md) 。

>[!IMPORTANT]
>
>購読プロセス中に、イベントプロバイダーとして **[!UICONTROL プラットフォーム通知]** (Platform notifications **[!UICONTROL )を選択し、プロンプトが表示されたら]** Data ingestion notificationイベント購読を選択します。

## データ取り込み通知の受信

Webフックの登録が完了し、新しいデータが取り込まれると、イベント通知の受信開始を設定できます。 これらのイベントは、Webフック自体を使用して表示するか、Adobe開発者コンソールでプロジェクトのイベント登録の概要にある[ **[!UICONTROL デバッグトレース]** ]タブを選択して表示できます。

以下のJSONは、バッチインジェストイベントが失敗した場合にWebフックに送信される通知ペイロードの例です。

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
| `event_id` | 通知用の、システム生成の一意のID。 |
| `event` | 通知をトリガーしたイベントの詳細を含むオブジェクトです。 |
| `event.xdm:datasetId` | インジェストイベントが適用されるデータセットのID。 |
| `event.xdm:eventCode` | データセットに対してトリガーされたイベントのタイプを示すステータスコード。 具体的な値とその定義については、 [付録](#event-codes) を参照してください。 |

イベント通知の完全なスキーマを表示するには、 [パブリックGitHubリポジトリを参照してください](https://github.com/adobe/xdm/blob/master/schemas/notifications/ingestion.schema.json)。

## 次の手順

プロジェクトに [!DNL Platform] 通知を登録すると、 [!UICONTROL プロジェクトの概要からイベントを受け取った表示を表示できます]。 Refer to the guide on [tracing Adobe I/O Events](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md) for detailed instructions on how to trace your events.

## 付録

次の節では、データインジェスト通知ペイロードの解釈に関する追加情報を説明します。

### 使用可能なステータス通知イベント {#event-codes}

次の表に、サブスクライブ可能なデータインジェストステータス通知をリストします。

| イベントコード | プラットフォームサービス | ステータス | イベントの説明 |
| --- | ---------------- | ------ | ----------------- |
| `ing_load_success` | [!DNL Data Ingestion] | 成功 | バッチが内のデータセットに正常に取り込まれました [!DNL Data Lake]。 |
| `ing_load_failure` | [!DNL Data Ingestion] | 失敗 | バッチを内のデータセットに取り込めませんでした [!DNL Data Lake]。 |
| `ps_load_success` | [!DNL Real-time Customer Profile] | 成功 | バッチが正常に [!DNL Profile] データストアに取り込まれました。 |
| `ps_load_failure` | [!DNL Real-time Customer Profile] | 失敗 | バッチをデータストアに取り込めませんでした [!DNL Profile] 。 |
| `ig_load_success` | [!DNL Identity Service] | 成功 | データが正常にIDグラフに読み込まれました。 |
| `ig_load_failure` | [!DNL Identity Service] | 失敗 | データをIDグラフに読み込めませんでした。 |

>[!NOTE]
>
> すべてのデータ取得通知に対して 1 つのイベントトピックのみが提供されます。異なるステータスを区別するために、イベントコードを使用できます。