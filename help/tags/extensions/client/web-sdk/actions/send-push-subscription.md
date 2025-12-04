---
title: プッシュ購読を送信
description: ブラウザープッシュ購読のデータを登録、送信、収集します。
source-git-commit: 3abe25a9c538bf4d1b439d48f624d8cad109a99e
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 7%

---

# プッシュ購読を送信

>[!AVAILABILITY]
>
>Web SDKのプッシュ通知は現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

**[!UICONTROL Send push subscription]** アクションは、プッシュ通知の購読をAdobe Experience Platformに登録します。 ブラウザーからのプッシュ購読の詳細の取得を処理し、設定済みのデータストリームに送信します。 Web SDK拡張機能バージョン 2.32.0 以降で使用できます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL Extension]」ドロップダウンフィールドを「**[!UICONTROL Adobe Experience Platform Web SDK]**」に設定し、「[!UICONTROL Action Type]」を「**[!UICONTROL Send push subscription]**」に設定します。

アクションの設定は、SDK インスタンスを選択するまでです。

このコマンドを使用する前に、拡張機能を設定する際には、有効な [VAPID 公開鍵 ](../configure/push-notifications.md) を設定していることを確認してください。

このアクションは、[`sendPushSubscription`](/help/collection/js/commands/sendpushsubscription.md) コマンドと同等のタグ拡張機能です。 前提条件、推奨される実行頻度、コマンドの動作方法およびエラー処理について詳しくは、リンクされたページを参照してください。

>[!MORELIKETHIS]
>
>* [プッシュ通知の設定](../configure/push-notifications.md)
>* [Web プッシュ API の仕様 ](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
>* [ サービスワーカー API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
