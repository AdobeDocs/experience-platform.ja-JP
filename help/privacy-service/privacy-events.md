---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: プライバシーに関するサービスのイベントの購読
topic-legacy: privacy events
description: 事前に構成された webhook を使用して、プライバシーサービスイベントを購読する方法について説明します。
exl-id: 9bd34313-3042-46e7-b670-7a330654b178
source-git-commit: 82dea48c732b3ddea957511c22f90bbd032ed9b7
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 15%

---

# 購読 [!DNL Privacy Service Events]

[!DNL Privacy Service Events] は、設定された [!DNL Privacy Service] webhook に送られる adobe I/o イベントを使用して、効率的なジョブ要求の自動化を促進する、Adobe 体験プラットフォームによって提供されているメッセージです。 ジョブが完了したか、またはワークフロー内の特定のマイルストーンに達したかを確認するために [!DNL Privacy Service] API をポーリングする必要性が削減される、または完全になくなります。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
| --- | --- |
| ジョブ完了 | すべての [!DNL Experience Cloud] アプリケーションが報告を送り、ジョブの全体または全体のステータスが完了状態になっていることを確認しました。 |
| ジョブエラー | 要求の処理中に、1つ以上のアプリケーションでエラーが報告されました。 |
| 製品完了 | このジョブに関連付けられたアプリケーションのいずれかの操作が完了しました。 |
| 製品エラー | 要求の処理中に、アプリケーションのいずれかがエラーを報告しました。 |

このドキュメントでは、通知用にイベント登録を設定 [!DNL Privacy Service] する手順と、通知ペイロードの解釈方法について説明します。

## はじめに

このチュートリアルを開始する前に、プライバシーに関する以下のマニュアルを参照してください。

* [Privacy Service の概要](./home.md)
* [プライバシーサービス API ガイド](./api/overview.md)

## Webhook をに登録します。 [!DNL Privacy Service Events]

受信するためには [!DNL Privacy Service Events] 、Adobe Developer Console を使用して、webhook を統合に登録する必要があり [!DNL Privacy Service] ます。

[  [!DNL I/O Event]  ](../observability/alerts/subscribe.md) この方法の詳細な手順については、通知の購読に関するチュートリアルを参照してください。**** 上記のイベントにアクセスするには、イベントプロバイダーとして「プライバシーサービス」のイベントを選択していることを確認してください。

## 通知の受信 [!DNL Privacy Service Event]

Webhook ジョブが正常に登録されたら、イベント通知の受信を開始できます。 これらのイベントを表示するには、webhook 自体を使用するか、 **** Adobe Developer Console でプロジェクトのイベント登録概要の「デバッグトレース」タブを選択します。

![](images/privacy-events/debug-tracing.png)

次の JSON は、 [!DNL Privacy Service Event] プライバシジョブに関連付けられたアプリケーションのいずれかが処理を完了したときに webhook に送信される通知ペイロードの例です。

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
| `id` | システムによって生成される通知の一意の ID です。 |
| `type` | 送信される通知のタイプ。で提供されている情報にコンテキストが表示され `data` ます。 次のような値を指定します。 <ul><li>`com.adobe.platform.gdpr.jobcomplete`</li><li>`com.adobe.platform.gdpr.joberror`</li><li>`com.adobe.platform.gdpr.productcomplete`</li><li>`com.adobe.platform.gdpr.producterror`</li></ul> |
| `time` | イベントが発生したときのタイムスタンプ。 |
| `data.value` | 通知をトリガーした対象に関する追加情報が含まれています。 <ul><li>`jobId`: 通知をトリガーしたプライバシージョブの ID。</li><li>`message`: ジョブの特定の状態に関するメッセージを表示します。 `productcomplete`または通知については、このフィールドには、対象となる `producterror` エクスペリエンスクラウドアプリケーションが表示されます。</li></ul> |

## 次の手順

このドキュメントでは、プライバシーサービスイベントを設定された webhook に登録する方法、および通知ペイロードの解釈方法について説明しました。 ユーザーインターフェイスを使用してプライバシージョブを追跡する方法については、 [ プライバシーサービスユーザーガイドを参照してください ](./ui/user-guide.md) 。
