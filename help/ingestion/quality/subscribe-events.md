---
keywords: Experience Platform；ホーム；人気の高いトピック；データ取り込み通知；通知；購読イベント；データ取り込みステータスイベント；ステータスイベント；購読；ステータス通知；
solution: Experience Platform
title: データ取得通知
description: 取り込みプロセスの監視に役立つように、Adobe Experience Platformでは、プロセスの各手順で公開された一連のイベントをサブスクライブし、取り込んだデータのステータスと考えられるエラーを通知できます。
exl-id: fd34e1ab-f6f6-44f0-88ee-7020e9322c39
source-git-commit: e802932dea38ebbca8de012a4d285eab691231be
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 28%

---

# データ取得通知

Adobe Experience Platform でデータを取得するプロセスは、複数の手順で構成されます。取り込む必要のあるデータファイルを特定したら、 [!DNL Platform]を指定した場合、取り込みプロセスが開始され、データが正常に取り込まれるか失敗するまで各手順が連続して実行されます。 取得処理は、[Adobe データ取得 API](https://www.adobe.io/experience-platform-apis/references/data-ingestion/) を使用するか、Experience Platform のユーザーインターフェイスを使用して開始することができます。[!DNL Experience Platform]

に読み込まれたデータ [!DNL Platform] 目的の場所に到達するには、複数の手順を実行する必要があります。 [!DNL Data Lake] または [!DNL Real-Time Customer Profile] データストア。 各手順では、データの処理やデータの検証が行われ、データが次の手順に渡される前にデータが保存されます。取得されるデータの量によっては、この処理に時間がかかる場合があり、検証、セマンティクスまたは処理エラーが原因でプロセスが失敗する可能性が常にあります。失敗した場合は、データの問題を修正し、修正したデータファイルを使用して取得プロセス全体を再開する必要があります。

取り込みプロセスの監視に役立つように、 [!DNL Experience Platform] を使用すると、プロセスの各手順で公開された一連のイベントを購読し、取り込んだデータのステータスと考えられるエラーを通知できます。

## データ取得通知用の Webhook の登録

データ取得通知を受け取るには、 [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) をクリックして、webhook をExperience Platform統合に登録します。

次のチュートリアルに従います。 [購読者 [!DNL Adobe I/O Event] 通知](../../observability/alerts/subscribe.md) を参照してください。

>[!IMPORTANT]
>
>購読プロセス中に、必ず **[!UICONTROL プラットフォーム通知]** をイベントプロバイダーとして選択し、 **[!UICONTROL データ取り込み通知]** 要求された場合はイベント購読。

## データ取得通知を受信

Webhook の登録が完了し、新しいデータが取り込まれたら、イベント通知の受信を開始できます。 これらのイベントは、Webhook 自体を使用して、または **[!UICONTROL デバッグトレース]** 」タブを使用します。

次の JSON は、バッチ取得イベントが失敗した場合に Webhook に送信される通知ペイロードの例です。

```json
{
  "event_id": "93a5b11a-b0e6-4b29-ad82-81b1499cb4f2",
  "event": {
    "xdm:ingestionId": "01EGK8H8HF9JGFKNDCABHGA24G",
    "xdm:customerIngestionId": "01EGK8H8HF9JGFKNDCABHGA24G",
    "xdm:imsOrg": "{ORG_ID}",
    "xdm:completed": 1598374341560,
    "xdm:datasetId": "5e55b556c2ae4418a8446037",
    "xdm:eventCode": "ing_load_failure",
    "xdm:sandboxName": "prod",
    "sentTime": "1598374341595",
    "processStartTime": 1598374342614,
    "transformedTime": 1598374342621,
    "header": {
      "_adobeio": {
        "imsOrgId": "{ORG_ID}",
        "providerMetadata": "aep_observability_catalog_events",
        "eventCode": "platform_event"
      }
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `event_id` | 通知用の、システムで生成された一意の ID。 |
| `event` | 通知をトリガーしたイベントの詳細を含むオブジェクト。 |
| `event.xdm:datasetId` | 取得イベントが適用されるデータセットの ID。 |
| `event.xdm:eventCode` | データセットに対してトリガーされたイベントのタイプを示すステータスコード。 詳しくは、 [付録](#event-codes) を参照してください。 |

イベント通知の完全なスキーマを表示するには、 [パブリック GitHub リポジトリ](https://github.com/adobe/xdm/blob/master/schemas/notifications/ingestion.schema.json).

## 次の手順

登録が完了したら、 [!DNL Platform] プロジェクトへの通知では、 [!UICONTROL プロジェクトの概要]. に関するガイドを参照してください。 [トレースAdobe I/Oイベント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md) イベントのトレース方法の詳細な手順については、を参照してください。

## 付録

次の節では、データ取得通知ペイロードの解釈に関する追加情報を示します。

### 使用可能なステータス通知イベント {#event-codes}

次の表に、サブスクライブできる、使用可能なデータ取得ステータス通知を示します。

| イベントコード | プラットフォームサービス | ステータス | イベントの説明 |
| --- | ---------------- | ------ | ----------------- |
| `ing_load_success` | [!DNL Data Ingestion] | 成功 | バッチが [!DNL Data Lake]. |
| `ing_load_failure` | [!DNL Data Ingestion] | 失敗 | バッチを内のデータセットに取り込めませんでした [!DNL Data Lake]. |
| `ps_load_success` | [!DNL Real-Time Customer Profile] | 成功 | バッチが [!DNL Profile] データストア。 |
| `ps_load_failure` | [!DNL Real-Time Customer Profile] | 失敗 | バッチをに取り込めませんでした [!DNL Profile] データストア。 |
| `ig_load_success` | [!DNL Identity Service] | 成功 | データは ID グラフに正常に読み込まれました。 |
| `ig_load_failure` | [!DNL Identity Service] | 失敗 | データを ID グラフに読み込めませんでした。 |

>[!NOTE]
>
> すべてのデータ取得通知に対して 1 つのイベントトピックのみが提供されます。異なるステータスを区別するために、イベントコードを使用できます。
