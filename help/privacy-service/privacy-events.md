---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Serviceイベントの購読
topic: privacy events
translation-type: tm+mt
source-git-commit: c5455dc0812b251483170ac19506d7c60ad4ecaa
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 7%

---


# 購読する [!DNL Privacy Service Events]

[!DNL Privacy Service Events] は、設定されたwebフックに送信されるAdobe I/Oのイベント [!DNL Privacy Service]を活用して、効率的なジョブリクエストの自動化を容易にする、Adobe Experience Platformから提供されるメッセージです。 They reduce or eliminate the need to poll the [!DNL Privacy Service] API in order to check if a job is complete or if a certain milestone within a workflow has been reached.

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
| --- | --- |
| ジョブ完了 | All [!DNL Experience Cloud] applications have reported back and the overall or global status of the job has been marked as complete. |
| ジョブエラー | 1つ以上のアプリケーションが、要求の処理中にエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているアプリの1つが作業を完了しました。 |
| 製品エラー | いずれかのアプリケーションが、要求の処理中にエラーを報告しました。 |

このドキュメントでは、通知のイベント登録を設定する手順と、通知ペイロードを解釈する方法について説明し [!DNL Privacy Service] ます。

## はじめに

このチュートリアルを開始する前に、次のPrivacy Serviceドキュメントを確認してください。

* [Privacy Service の概要](./home.md)
* [Privacy ServiceAPI開発者ガイド](./api/getting-started.md)

## Webフックの登録先 [!DNL Privacy Service Events]

を受け取るに [!DNL Privacy Service Events]は、Adobeデベロッパーコンソールを使用してWebフックを [!DNL Privacy Service] 統合に登録する必要があります。

これを行う方法について詳しくは、 [購読に関するチュートリアルに従って [!DNL I/O Event] 通知を行います](../observability/notifications/subscribe.md) 。 上記のイベントにアクセスするには、 **[!UICONTROL Privacy Serviceイベント]** (イベントプロバイダー)を選択していることを確認してください。

## 通知の受信 [!DNL Privacy Service Event]

Webフックの登録とプライバシージョブの実行が完了すると、イベント通知の受信開始を設定できます。 これらのイベントは、Webフック自体を使用して表示するか、Adobe開発者コンソールでプロジェクトのイベント登録の概要にある[ **[!UICONTROL デバッグトレース]** ]タブを選択して表示できます。

![](images/privacy-events/debug-tracing.png)

次のJSONは、プライバシージョブに関連付けられたアプリケーションの1つがその作業を完了した場合にWebフックに送信される [!DNL Privacy Service Event] 通知ペイロードの例です。

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
| `id` | 通知用の、システム生成の一意のID。 |
| `type` | 送信される通知のタイプ。で提供される情報にコンテキストを与え `data`ます。 有効な値は次のとおりです。 <ul><li>`com.adobe.platform.gdpr.jobcomplete`</li><li>`com.adobe.platform.gdpr.joberror`</li><li>`com.adobe.platform.gdpr.productcomplete`</li><li>`com.adobe.platform.gdpr.producterror`</li></ul> |
| `time` | イベントが発生した日時のタイムスタンプ。 |
| `data.value` | 通知のトリガー元に関する追加情報が含まれます。 <ul><li>`jobId`:通知をトリガーしたプライバシージョブのID。</li><li>`message`:ジョブの特定のステータスに関するメッセージです。 通知の場合 `productcomplete` または `producterror` 通知の場合、このフィールドには問題のExperience Cloudアプリケーションが示されます。</li></ul> |

## 次の手順

このドキュメントでは、設定済みのWebフックにPrivacy Serviceイベントを登録する方法、および通知ペイロードを解釈する方法について説明します。 ユーザーインターフェイスを使用してプライバシージョブを追跡する方法については、 [Privacy Serviceユーザーガイドを参照してください](./ui/user-guide.md)。