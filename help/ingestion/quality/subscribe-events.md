---
keywords: Experience Platform；ホーム；人気のトピック；データ取得通知；通知；サブスクライブイベント；データ取得ステータスイベント；ステータスイベント；サブスクライブ；ステータス通知；
solution: Experience Platform
title: データ取得通知
description: 取り込みプロセスの監視を支援するために、Adobe Experience Platformでは、プロセスの各ステップで公開される一連のイベントを登録し、取り込んだデータのステータスと発生する可能性のあるエラーを通知できるようにしています。
exl-id: fd34e1ab-f6f6-44f0-88ee-7020e9322c39
source-git-commit: 76ef5638316a89aee1c6fb33370af943228b75e1
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 20%

---

# データ取得通知

Adobe Experience Platform でデータを取得するプロセスは、複数の手順で構成されます。[!DNL Platform] に取り込む必要があるデータファイルを特定すると、取り込みプロセスが開始され、データが正常に取り込まれるか失敗するまで、各手順が連続して実行されます。 取り込みプロセスは、[Adobe Experience Platform バッチ取り込み API](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/) または [!DNL Experience Platform] ユーザーインターフェイスを使用して開始できます。

[!DNL Platform] に読み込むデータが宛先、[!DNL Data Lake] または [!DNL Real-Time Customer Profile] データストアに到達するには、複数の手順を経る必要があります。 各手順では、データの処理やデータの検証が行われ、データが次の手順に渡される前にデータが保存されます。取得されるデータの量によっては、この処理に時間がかかる場合があり、検証、セマンティクスまたは処理エラーが原因でプロセスが失敗する可能性が常にあります。失敗した場合は、データの問題を修正し、修正したデータファイルを使用して取得プロセス全体を再開する必要があります。

取り込みプロセスの監視を支援するために、[!DNL Experience Platform] では、プロセスの各ステップで公開される一連のイベントを登録し、取り込んだデータのステータスと発生する可能性のあるエラーを通知できるようにしています。

## データ取得通知の Webhook を登録

データ取得通知を受け取るには、[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) を使用して、Experience Platform統合に Webhook を登録する必要があります。

これを実行する方法に関する詳細な手順については、[ 購読  [!DNL Adobe I/O Event]  通知 ](../../observability/alerts/subscribe.md) のチュートリアルに従ってください。

>[!IMPORTANT]
>
>購読プロセス中に、必ず **[!UICONTROL Platform 通知]** をイベントプロバイダーとして選択し、プロンプトが表示されたら **[!UICONTROL データ取り込み通知]** イベント購読を選択します。

## データ取得通知を受信

Webhook を正常に登録し、新しいデータが取り込まれたら、イベント通知の受信を開始できます。 これらのイベントは、Webhook 自体を使用するか、Adobe Developer Consoleでプロジェクトのイベント登録の概要にある「**[!UICONTROL デバッグトレース]**」タブを選択すると表示できます。

次の JSON は、バッチ取り込みイベントに失敗した場合に Webhook に送信される通知ペイロードの例です。

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
| `event_id` | 通知の一意の、システムで生成された ID。 |
| `event` | 通知をトリガーしたイベントの詳細を含むオブジェクト。 |
| `event.xdm:datasetId` | 取り込みイベントが適用されるデータセットの ID。 |
| `event.xdm:eventCode` | データセットに対してトリガーされたイベントのタイプを示すステータスコード。 特定の値とその定義については、[ 付録 ](#event-codes) を参照してください。 |

イベント通知の完全なスキーマを表示するには、[ 公開 GitHub リポジトリ ](https://github.com/adobe/xdm/blob/master/schemas/notifications/ingestion.schema.json) を参照してください。

## 次の手順

プロジェクトに [!DNL Platform] 通知を登録すると、[!UICONTROL  プロジェクトの概要 ] から受信したイベントを表示できます。 イベントをトレースする方法について詳しくは、[Adobe I/Oイベントのトレース ](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md) に関するガイドを参照してください。

## 付録

次の節では、データ取得通知ペイロードの解釈に関する追加情報を示します。

### 使用可能なステータス通知イベント {#event-codes}

次の表に、購読できる使用可能なデータ取り込みステータス通知を示します。

| イベントコード | プラットフォームサービス | ステータス | イベントの説明 |
| --- | ---------------- | ------ | ----------------- |
| `ing_load_success` | [!DNL Data Ingestion] | 成功 | バッチが [!DNL Data Lake] 内のデータセットに正常に取り込まれました。 |
| `ing_load_failure` | [!DNL Data Ingestion] | 失敗 | バッチを [!DNL Data Lake] 内のデータセットに取り込めませんでした。 |
| `ps_load_success` | [!DNL Real-Time Customer Profile] | 成功 | バッチが [!DNL Profile] データストアに正常に取り込まれました。 |
| `ps_load_failure` | [!DNL Real-Time Customer Profile] | 失敗 | バッチを [!DNL Profile] データストアに取り込めませんでした。 |
| `ig_load_success` | [!DNL Identity Service] | 成功 | データが ID グラフに正常に読み込まれました。 |
| `ig_load_failure` | [!DNL Identity Service] | 失敗 | データを ID グラフに読み込めませんでした。 |

>[!NOTE]
>
>すべてのデータ取得通知に対して提供されるイベントトピックは 1 つだけです。 異なるステータスを区別するために、イベントコードを使用できます。
