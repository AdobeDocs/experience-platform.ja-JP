---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy Serviceイベントの登録
description: 事前設定済みの Webhook を使用してPrivacy Serviceイベントを登録する方法について説明します。
exl-id: 9bd34313-3042-46e7-b670-7a330654b178
source-git-commit: 0f7ef438db5e7141197fb860a5814883d31ca545
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 8%

---

# [!DNL Privacy Service Events] を購読

Adobe Experience Platform [!DNL Privacy Service] が提供するメッセージで、設定済みの Webhook に送信されるAdobe I/Oイベントを活用して、効率的なジョブリクエストの自動処理を容易にする [!DNL Privacy Service Events] のがあります。 ジョブが完了したか、またはワークフロー内の特定のマイルストーンに達したかを確認するために [!DNL Privacy Service] API をポーリングする必要性が削減される、または完全になくなります。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
| --- | --- |
| ジョブ完了 | すべての [!DNL Experience Cloud] アプリケーションがレポートを返し、ジョブの全体的またはグローバルなステータスが完了としてマークされました。 |
| ジョブエラー | 1 つ以上のアプリケーションが要求の処理中にエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているアプリケーションの 1 つが作業を完了しました。 |
| 製品エラー | いずれかのアプリケーションが要求の処理中にエラーを報告しました。 |

このドキュメントでは、[!DNL Privacy Service] 通知のイベント登録を設定する手順と、通知ペイロードを解釈する方法について説明します。

## はじめに

このチュートリアルを開始する前に、次のPrivacy Serviceドキュメントを確認してください。

* [Privacy Service の概要](./home.md)
* [Privacy ServiceAPI ガイド](./api/overview.md)

## [!DNL Privacy Service Events] への Webhook の登録

[!DNL Privacy Service Events] を受け取るには、Adobe Developer Consoleを使用して、[!DNL Privacy Service] 統合に Webhook を登録する必要があります。

これを実現する方法の手順について詳しくは、[[!DNL I/O イベント &#x200B;] 通知のサブスクライブ ](../observability/alerts/subscribe.md) に関するチュートリアルに従ってください。 上記のイベントにアクセスするには、イベントプロバイダーとして **0&rbrace;Privacy Serviceイベント &rbrace; を選択してください。**

## [!DNL Privacy Service Event] 通知を受信

Webhook を正常に登録し、プライバシージョブが実行されたら、イベント通知の受信を開始できます。 これらのイベントは、Webhook 自体を使用するか、Adobe Developer Consoleでプロジェクトのイベント登録の概要にある「**[!UICONTROL デバッグトレース]**」タブを選択すると表示できます。

![](images/privacy-events/debug-tracing.png)

次の JSON は、プライバシージョブに関連付けられたいずれかのアプリケーションが作業を完了した際に Webhook に送信される [!DNL Privacy Service Event] 通知ペイロードの例を示しています。

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
| `id` | 通知の一意の、システムで生成された ID。 |
| `type` | 送信される通知のタイプ。`data` で提供される情報にコンテキストを提供します。 次のような値が考えられます。 <ul><li>`com.adobe.platform.gdpr.jobcomplete`</li><li>`com.adobe.platform.gdpr.joberror`</li><li>`com.adobe.platform.gdpr.productcomplete`</li><li>`com.adobe.platform.gdpr.producterror`</li></ul> |
| `time` | イベントが発生したときのタイムスタンプ。 |
| `data.value` | 通知をトリガーした内容に関する追加情報が含まれます。 <ul><li>`jobId`：通知をトリガーしたプライバシージョブの ID。</li><li>`message`：ジョブの特定のステータスに関するメッセージ。 `productcomplete` または `producterror` の通知の場合、このフィールドは、問題のExperience Cloudアプリケーションを示します。</li></ul> |

## 次の手順

このドキュメントでは、設定済みの Webhook にPrivacy Serviceイベントを登録する方法と、通知ペイロードを解釈する方法について説明しました。 ユーザーインターフェイスを使用してプライバシージョブをトラッキングする方法については、[Privacy Serviceユーザーガイド ](./ui/user-guide.md) を参照してください。
