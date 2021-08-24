---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy Serviceイベントの購読
topic-legacy: privacy events
description: 事前設定済みのWebhookを使用してPrivacy Serviceイベントに登録する方法を説明します。
exl-id: 9bd34313-3042-46e7-b670-7a330654b178
source-git-commit: a455134a45137b171636d6525ce9124bc95f4335
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 15%

---

# [!DNL Privacy Service Events]に登録

[!DNL Privacy Service Events] は、設定済みのWebhookに送信されるAdobe I/Oイベントを活用し [!DNL Privacy Service]て、効率的なジョブリクエストの自動化を促進する、Adobe Experience Platformが提供するメッセージです。ジョブが完了したか、またはワークフロー内の特定のマイルストーンに達したかを確認するために [!DNL Privacy Service] API をポーリングする必要性が削減される、または完全になくなります。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
| --- | --- |
| ジョブ完了 | すべての[!DNL Experience Cloud]アプリケーションがレポートを返し、ジョブの全体的なステータスまたはグローバルステータスが完了とマークされました。 |
| ジョブエラー | リクエストの処理中に1つ以上のアプリケーションがエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているアプリケーションの1つが、作業を完了しました。 |
| 製品エラー | リクエストの処理中に、1つのアプリケーションがエラーを報告しました。 |

このドキュメントでは、[!DNL Privacy Service]通知のイベント登録を設定する手順と、通知ペイロードの解釈方法を説明します。

## はじめに

このチュートリアルを開始する前に、次のPrivacy Serviceドキュメントを確認してください。

* [Privacy Service の概要](./home.md)
* [Privacy ServiceAPI開発者ガイド](./api/getting-started.md)

## [!DNL Privacy Service Events]へのWebhookの登録

[!DNL Privacy Service Events]を受け取るには、Adobe開発者コンソールを使用して、Webhookを[!DNL Privacy Service]統合に登録する必要があります。

これをおこなう方法の詳細な手順については、[ [!DNL I/O Event] 通知](../observability/alerts/subscribe.md)の購読に関するチュートリアルを参照してください。 上記のイベントにアクセスするには、Privacy Serviceプロバイダーとして&#x200B;**[!UICONTROL イベントイベント]**&#x200B;を選択してください。

## [!DNL Privacy Service Event]通知を受信

Webhookの登録が完了し、プライバシージョブが実行されたら、イベント通知の受信を開始できます。 これらのイベントは、Webhook自体を使用するか、Adobe開発者コンソールでプロジェクトのイベント登録の概要の「**[!UICONTROL デバッグトレース]**」タブを選択して表示できます。

![](images/privacy-events/debug-tracing.png)

次のJSONは、プライバシージョブに関連付けられたアプリケーションの1つがその作業を完了したときにWebhookに送信される[!DNL Privacy Service Event]通知ペイロードの例です。

```json
{
  "id":"b472e249-368b-4706-90f3-1d774713f827",
  "event_id":"b116f797-e50b-432e-9c65-189106a34820",
  "specversion":"0.2",
  "type":"com.adobe.platform.gdpr.productcomplete",
  "source":"https://ns.adobe.com/platform/gdpr",
  "time":"Wed Oct 23 18:52:32 GMT 2019",
  "data":{
    "imsOrg":"{IMS_ORG}",
    "value":{
      "jobId":"6f0f2b62-88a7-4515-ba05-432d9a7021c5",
      "message":"analytics.access.complete"
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 通知用の、システムで生成された一意のID。 |
| `type` | 送信される通知のタイプ。`data`で提供される情報にコンテキストを与えます。 有効な値は次のとおりです。 <ul><li>`com.adobe.platform.gdpr.jobcomplete`</li><li>`com.adobe.platform.gdpr.joberror`</li><li>`com.adobe.platform.gdpr.productcomplete`</li><li>`com.adobe.platform.gdpr.producterror`</li></ul> |
| `time` | イベントが発生した日時のタイムスタンプ。 |
| `data.value` | 通知をトリガーした理由に関する追加情報が含まれます。 <ul><li>`jobId`:通知をトリガーしたプライバシージョブのID。</li><li>`message`:ジョブの特定のステータスに関するメッセージ。`productcomplete`または`producterror`通知の場合、このフィールドは問題のExperience Cloudアプリケーションを示します。</li></ul> |

## 次の手順

このドキュメントでは、設定済みのWebhookにPrivacy Serviceイベントを登録する方法と、通知ペイロードの解釈方法について説明します。 ユーザーインターフェイスを使用してプライバシージョブを追跡する方法については、『[Privacy Serviceユーザーガイド](./ui/user-guide.md)』を参照してください。
