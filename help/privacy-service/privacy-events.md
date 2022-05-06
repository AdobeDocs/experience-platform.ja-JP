---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy Serviceイベントに登録
topic-legacy: privacy events
description: 事前設定済みの Webhook を使用してPrivacy Serviceイベントに登録する方法を説明します。
exl-id: 9bd34313-3042-46e7-b670-7a330654b178
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 15%

---

# 配信登録 [!DNL Privacy Service Events]

[!DNL Privacy Service Events] は、Adobe Experience Platformが提供するメッセージです [!DNL Privacy Service]：設定済みの Webhook に送信されたAdobe I/Oイベントを活用して、効率的なジョブリクエストの自動化を促進します。 ジョブが完了したか、またはワークフロー内の特定のマイルストーンに達したかを確認するために [!DNL Privacy Service] API をポーリングする必要性が削減される、または完全になくなります。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
| --- | --- |
| ジョブ完了 | すべて [!DNL Experience Cloud] アプリケーションからレポートが返され、ジョブの全体的なステータスまたはグローバルステータスが完了とマークされました。 |
| ジョブエラー | リクエストの処理中に 1 つ以上のアプリケーションがエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているアプリケーションの 1 つが、作業を完了しました。 |
| 製品エラー | リクエストの処理中に 1 つのアプリケーションがエラーを報告しました。 |

このドキュメントでは、 [!DNL Privacy Service] 通知と、通知ペイロードの解釈方法を示します。

## はじめに

このチュートリアルを開始する前に、次のPrivacy Serviceドキュメントを確認してください。

* [Privacy Service の概要](./home.md)
* [Privacy ServiceAPI ガイド](./api/overview.md)

## ウェブフックの登録先 [!DNL Privacy Service Events]

受け取る [!DNL Privacy Service Events]を使用している場合、Webhook を [!DNL Privacy Service] 統合とも呼ばれます。

次のチュートリアルに従います。 [[!DNL I/O Event] 通知の購読](../observability/alerts/subscribe.md) を参照してください。 必ず **[!UICONTROL Privacy Serviceイベント]** 上記のイベントにアクセスするには、をイベントプロバイダーとして設定します。

## 受信 [!DNL Privacy Service Event] 通知

Webhook の登録とプライバシージョブの実行が完了したら、イベント通知の受信を開始できます。 これらのイベントは、Webhook 自体を使用して、または **[!UICONTROL デバッグトレース]** 」タブを使用します。

![](images/privacy-events/debug-tracing.png)

次の JSON は、 [!DNL Privacy Service Event] プライバシージョブに関連付けられたアプリケーションの 1 つがその作業を完了したときに webhook に送信される通知ペイロード：

```json
{
  "id":"b472e249-368b-4706-90f3-1d774713f827",
  "event_id":"b116f797-e50b-432e-9c65-189106a34820",
  "specversion":"0.2",
  "type":"com.adobe.platform.gdpr.productcomplete",
  "source":"https://ns.adobe.com/platform/gdpr",
  "time":"Wed Oct 23 18:52:32 GMT 2019",
  "data":{
    "imsOrg":"{ORG_ID}",
    "value":{
      "jobId":"6f0f2b62-88a7-4515-ba05-432d9a7021c5",
      "message":"analytics.access.complete"
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 通知用の、システムで生成された一意の ID。 |
| `type` | 送信される通知のタイプ。 `data`. 有効な値は次のとおりです。 <ul><li>`com.adobe.platform.gdpr.jobcomplete`</li><li>`com.adobe.platform.gdpr.joberror`</li><li>`com.adobe.platform.gdpr.productcomplete`</li><li>`com.adobe.platform.gdpr.producterror`</li></ul> |
| `time` | イベントが発生した日時のタイムスタンプ。 |
| `data.value` | 通知をトリガーしたアイテムに関する追加情報が含まれます。 <ul><li>`jobId`:通知をトリガーしたプライバシージョブの ID。</li><li>`message`:ジョブの特定のステータスに関するメッセージ。 の場合 `productcomplete` または `producterror` 通知。このフィールドは、該当するExperience Cloud・アプリケーションを示します。</li></ul> |

## 次の手順

このドキュメントでは、設定済みの Webhook にPrivacy Serviceイベントを登録する方法と、通知ペイロードの解釈方法について説明しました。 ユーザーインターフェイスを使用してプライバシージョブを追跡する方法については、 [Privacy Serviceユーザーガイド](./ui/user-guide.md).
