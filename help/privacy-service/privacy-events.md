---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Privacy Serviceイベントの購読
topic-legacy: privacy events
description: 事前に設定されたWebフックを使用してPrivacy Serviceイベントを登録する方法を学びます。
exl-id: 9bd34313-3042-46e7-b670-7a330654b178
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 7%

---

# [!DNL Privacy Service Events]に登録

[!DNL Privacy Service Events] は、設定済みのwebフックに送信されるAdobe I/Oイベントを活用して、効率的なジョブリクエストの自動化を促進する、Adobe Experience Platformが提供するメッセージです。 [!DNL Privacy Service]ジョブが完了したか、またはワークフロー内の特定のマイルストーンに達したかを確認するために、[!DNL Privacy Service] APIをポーリングする必要が少なくなるか、なくなります。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
| --- | --- |
| ジョブ完了 | すべての[!DNL Experience Cloud]アプリケーションが報告を返し、ジョブの全体的な状態またはグローバル状態が完了とマークされました。 |
| ジョブエラー | 1つ以上のアプリケーションが、要求の処理中にエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているアプリの1つが作業を完了しました。 |
| 製品エラー | いずれかのアプリケーションが、要求の処理中にエラーを報告しました。 |

このドキュメントでは、[!DNL Privacy Service]通知のイベント登録を設定する手順と、通知ペイロードを解釈する方法について説明します。

## はじめに

このチュートリアルを開始する前に、次のPrivacy Serviceドキュメントを確認してください。

* [Privacy Service の概要](./home.md)
* [Privacy ServiceAPI開発者ガイド](./api/getting-started.md)

## Webフックを[!DNL Privacy Service Events]に登録

[!DNL Privacy Service Events]を受け取るには、Adobeデベロッパーコンソールを使用してWebフックを[!DNL Privacy Service]統合に登録する必要があります。

これを行う方法の詳細な手順については、[ [!DNL I/O Event] 通知](../observability/notifications/subscribe.md)のサブスクライブのチュートリアルに従ってください。 上記のイベントにアクセスするには、イベントプロバイダーとして&#x200B;**[!UICONTROL Privacy Serviceイベント]**&#x200B;を選択してください。

## [!DNL Privacy Service Event]通知を受信

Webフックの登録とプライバシージョブの実行が完了すると、イベント通知の受信開始を設定できます。 これらのイベントは、Webフック自体を使用して表示するか、Adobe開発者コンソールでプロジェクトのイベント登録の概要にある[**[!UICONTROL Debug Tracing]**]タブを選択して表示できます。

![](images/privacy-events/debug-tracing.png)

次のJSONは、プライバシージョブに関連付けられたアプリケーションの1つがその作業を完了した場合にWebフックに送信される[!DNL Privacy Service Event]通知ペイロードの例です。

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
| `type` | 送信される通知のタイプ。`data`で提供される情報にコンテキストを与えます。 有効な値は次のとおりです。 <ul><li>`com.adobe.platform.gdpr.jobcomplete`</li><li>`com.adobe.platform.gdpr.joberror`</li><li>`com.adobe.platform.gdpr.productcomplete`</li><li>`com.adobe.platform.gdpr.producterror`</li></ul> |
| `time` | イベントが発生した日時のタイムスタンプ。 |
| `data.value` | 通知のトリガー元に関する追加情報が含まれます。 <ul><li>`jobId`:通知をトリガーしたプライバシージョブのID。</li><li>`message`:ジョブの特定のステータスに関するメッセージです。`productcomplete`または`producterror`通知の場合、このフィールドは対象となるExperience Cloudアプリケーションを示します。</li></ul> |

## 次の手順

このドキュメントでは、設定済みのWebフックにPrivacy Serviceイベントを登録する方法、および通知ペイロードを解釈する方法について説明します。 ユーザーインターフェイスを使用してプライバシージョブを追跡する方法については、[Privacy Serviceユーザーガイド](./ui/user-guide.md)を参照してください。
