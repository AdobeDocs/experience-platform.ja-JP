---
keywords: Experience Platform;home;popular topics;date range
solution: Experience Platform
title: Adobe I/Oイベントの通知を購読する
topic: developer guide
description: このドキュメントでは、Adobe Experience PlatformサービスのAdobe I/Oイベント通知を登録する方法について手順を説明します。 使用可能なイベントタイプに関する参照情報と、各 [!DNL Platform] applicableserviceで返されたイベントデータの解釈方法に関する詳細ドキュメントへのリンクも記載されています。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 4%

---


# Adobe I/Oイベントの通知を購読する

[!DNL Observability Insights] adobe experience platformアクティビティに関するAdobe I/Oイベントの通知を購読できます。 これらのイベントは、アクティビティ監視の効率的な自動化を容易にするために、設定済みのWebフックに送信されます。

このドキュメントでは、Adobe Experience PlatformサービスのAdobe I/Oイベント通知を登録する方法について手順を説明します。 利用可能なイベントタイプに関する参照情報と、該当する各 [!DNL Platform] サービスで返されたイベントデータを解釈する方法に関する詳細なドキュメントへのリンクも記載されています。

## はじめに

このドキュメントでは、Webフックに関する実際の理解と、あるアプリケーションから別のアプリケーションへのWebフックの接続方法を理解する必要があります。 Webhooksの概要については、 [[!DNL I/O Events] ドキュメント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md) を参照してください。

## Webフックの作成

通知を受信するには、イベント登録の詳細の一部として一意のWebフックURLを指定して、Webフックを登録する必要があります。 [!DNL I/O Event]

Webフックは、選択したクライアントを使用して設定できます。 このチュートリアルで使用する一時的なWebフックアドレスについては、 [Webhook.siteにアクセスし](https://webhook.site/) 、提供された一意のURLをコピーしてください。

![](../images/notifications/webhook-url.png)

初期検証プロセス中に、GETリクエスト内の [!DNL I/O Events]`challenge` クエリパラメーターをWebフックに送信します。 応答ペイロードでこのパラメーターの値を返すようにWebフックを設定する必要があります。 Webhook.siteを使用している場合は、右上隅 **[!DNL Edit]** のを選択し、を入力してからを `$request.query.challenge$` 選択し **[!DNL Response body]****[!DNL Save]**&#x200B;ます。

![](../images/notifications/response-challenge.png)

## Adobeデベロッパーコンソールでの新しいプロジェクトの作成

[Adobeデベロッパーコンソールに移動し](https://www.adobe.com/go/devs_console_ui) 、Adobe IDでサインインします。 次に、Adobe開発者コンソールのドキュメントで、空のプロジェクトの [作成に関するチュートリアルに説明されている手順に従います](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) 。

## イベントの購読

新しいプロジェクトを作成したら、そのプロジェクトの概要画面に移動します。 ここから「 **[!UICONTROL イベント追加」を選択します]**。

![](../images/notifications/add-event-button.png)

イベントプロバイダーをプロジェクトに追加できるダイアログが表示されます。

* 通知をサブスクライブする場合は、「 [!DNL Experience Platform] プラット **[!UICONTROL フォーム通知」を選択します]**
* Adobe Experience Platform [!DNL Privacy Service] 通知をサブスクライブする場合は、「 **[!UICONTROL Privacy Serviceイベント」を選択します]**

イベントプロバイダーを選択したら、「 **[!UICONTROL 次へ]**」を選択します。

![](../images/notifications/event-provider.png)

次の画面には、登録するイベントタイプのリストが表示されます。 購読するイベントを選択し、「 **[!UICONTROL 次へ]**」を選択します。

>[!NOTE]
>
>使用しているサービスを購読するイベントが不明な場合は、次のサービス固有の通知ドキュメントを参照してください。
>
>* [[!DNL Privacy Service] 通知](../../privacy-service/privacy-events.md)
>* [[!DNL Data Ingestion] 通知](../../ingestion/quality/subscribe-events.md)
>* [[!DNL Flow Service (sources)] 通知](../../sources/notifications.md)


![](../images/notifications/choose-event-subscriptions.png)

次の画面では、JSON Web Token(JWT)の作成を求めるプロンプトが表示されます。 自動的にキーペアを生成するか、端末で生成した独自の公開鍵をアップロードするかを選択できます。

このチュートリアルでは、最初のオプションに従います。 「キーペアを **[!UICONTROL 生成」のオプションボックスを選択し]**、右下隅の「キーペアを **[!UICONTROL 生成]** 」ボタンを選択します。

![](../images/notifications/generate-keypair.png)

キーペアが生成されると、ブラウザーによって自動的にダウンロードされます。 このファイルは開発者コンソールで保持されないので、自分で保存する必要があります。

次の画面では、新しく生成されたキーペアの詳細を確認できます。 Select **[!UICONTROL Next]** to continue.

![](../images/notifications/keypair-generated.png)

次の画面で、「 [!UICONTROL イベント登録の詳細] 」セクションにイベント登録の名前と説明を入力します。 ベストプラクティスは、同じプロジェクト内の他のユーザーとこのイベントの登録を区別できるように、一意で、簡単に識別できる名前を作成することです。

![](../images/notifications/registration-details.png)

同じ画面の「イベントの受信 [!UICONTROL 方法] 」セクションの下で、必要に応じてイベントの受信方法を設定できます。 **[!UICONTROL Webhook]** では、イベントを受け取るカスタムWebフックアドレスを指定できますが、 **[!UICONTROL Runtime action]** では [Adobe I/O Runtimeを使用して同じことを行えます](https://www.adobe.io/apis/experienceplatform/runtime/docs.html)。

このチュートリアルでは、 **[!UICONTROL Webhook]** を選択し、前に作成したWebフックのURLを指定します。 完了したら、「設定済みのイベントを **[!UICONTROL 保存]** 」を選択してイベントの登録を完了します。

![](../images/notifications/receive-events.png)

新しく作成されたイベント登録の詳細ページが表示されます。このページでは、設定の編集、受信したイベントの確認、デバッグトレースの実行、新しいイベントプロバイダーの追加を行うことができます。

![](../images/notifications/registration-complete.png)

## 次の手順

このチュートリアルに従って、および/またはの通知を受信するWebフックを登録し [!DNL I/O Event] ま [!DNL Experience Platform][!DNL Privacy Service]した。 使用可能なイベントーと各サービスの通知ペイロードの解釈方法について詳しくは、次のドキュメントを参照してください。

* [[!DNL Privacy Service] 通知](../../privacy-service/privacy-events.md)
* [[!DNL Data Ingestion] 通知](../../ingestion/quality/subscribe-events.md)
* [[!DNL Flow Service (sources)] 通知](../../sources/notifications.md)

およびのアクティビティを監視する方法について詳しくは、 [[!DNL Observability Insights] 概要](../home.md) を参照し [!DNL Experience Platform] てくだ [!DNL Privacy Service]さい。