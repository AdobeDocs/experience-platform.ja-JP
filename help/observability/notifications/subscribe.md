---
keywords: Experience Platform；ホーム；人気のあるトピック；日付範囲
solution: Experience Platform
title: Adobe I/Oイベント通知のサブスクライブ
topic-legacy: developer guide
description: このドキュメントでは、Adobe Experience PlatformサービスのAdobe I/Oイベント通知を登録する方法について手順を説明します。 使用可能なイベントタイプに関するリファレンス情報と、該当する [!DNL Platform] サービスごとに返されたイベントデータを解釈する方法に関する詳細なドキュメントへのリンクも提供されています。
exl-id: c0ad7217-ce84-47b0-abf6-76bcf280f026
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 4%

---

# Adobe I/Oイベント通知のサブスクライブ

[!DNL Observability Insights] Adobe Experience Platformアクティビティに関するAdobe I/Oイベント通知を購読できます。これらのイベントは、アクティビティ監視の効率的な自動化を容易にするために、設定済みのWebフックに送信されます。

このドキュメントでは、Adobe Experience PlatformサービスのAdobe I/Oイベント通知を登録する方法について手順を説明します。 利用可能なイベントタイプに関するリファレンス情報と、該当する[!DNL Platform]サービスごとに返されたイベントデータを解釈する方法に関する詳細なドキュメントへのリンクも提供されています。

## はじめに

このドキュメントでは、Webフックに関する実際の理解と、あるアプリケーションから別のアプリケーションへのWebフックの接続方法を理解する必要があります。 Webhookの紹介については、[[!DNL I/O Events] ドキュメント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)を参照してください。

## Webフックの作成

[!DNL I/O Event]通知を受け取るには、イベント登録の詳細の一部として一意のWebフックURLを指定して、Webフックを登録する必要があります。

Webフックは、選択したクライアントを使用して設定できます。 このチュートリアルで使用する一時的なWebフックアドレスについては、[Webhook.site](https://webhook.site/)にアクセスし、提供された一意のURLをコピーしてください。

![](../images/notifications/webhook-url.png)

初期検証プロセス中、[!DNL I/O Events]は、GETリクエスト内の`challenge`クエリパラメーターをWebフックに送信します。 応答ペイロードでこのパラメーターの値を返すようにWebフックを設定する必要があります。 Webhook.siteを使用している場合は、右上隅の&#x200B;**[!DNL Edit]**&#x200B;を選択し、**[!DNL Save]**&#x200B;を選択する前に&#x200B;**[!DNL Response body]**&#x200B;の下に`$request.query.challenge$`と入力します。

![](../images/notifications/response-challenge.png)

## Adobeデベロッパーコンソールでの新しいプロジェクトの作成

[Adobeデベロッパーコンソール](https://www.adobe.com/go/devs_console_ui)に移動し、Adobe IDでサインインします。 次に、Adobe開発者コンソールドキュメントの[空のプロジェクト](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)の作成に関するチュートリアルに説明されている手順に従います。

## イベントの購読

新しいプロジェクトを作成したら、そのプロジェクトの概要画面に移動します。 ここから、**[!UICONTROL 追加イベント]**&#x200B;を選択します。

![](../images/notifications/add-event-button.png)

イベントプロバイダーをプロジェクトに追加できるダイアログが表示されます。

* [!DNL Experience Platform]通知をサブスクライブする場合は、「**[!UICONTROL プラットフォーム通知]**」を選択します
* Adobe Experience Platform[!DNL Privacy Service]通知をサブスクライブする場合は、**[!UICONTROL Privacy Serviceイベント]**&#x200B;を選択します

イベントプロバイダーを選択したら、「**[!UICONTROL 次へ]**」を選択します。

![](../images/notifications/event-provider.png)

次の画面には、登録するイベントタイプのリストが表示されます。 購読するイベントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択します。

>[!NOTE]
>
>使用しているサービスを購読するイベントが不明な場合は、次のサービス固有の通知ドキュメントを参照してください。
>
>* [[!DNL Privacy Service] 通知](../../privacy-service/privacy-events.md)
>* [[!DNL Data Ingestion] 通知](../../ingestion/quality/subscribe-events.md)
>* [[!DNL Flow Service (sources)] 通知](../../sources/notifications.md)


![](../images/notifications/choose-event-subscriptions.png)

次の画面では、JSON Web Token(JWT)の作成を求めるプロンプトが表示されます。 自動的にキーペアを生成するか、端末で生成した独自の公開鍵をアップロードするかを選択できます。

このチュートリアルでは、最初のオプションに従います。 「**[!UICONTROL キーペアを生成]**」のオプションボックスを選択し、右下の「**[!UICONTROL キーペアを生成]**」ボタンを選択します。

![](../images/notifications/generate-keypair.png)

キーペアが生成されると、ブラウザーによって自動的にダウンロードされます。 このファイルは開発者コンソールで保持されないので、自分で保存する必要があります。

次の画面では、新しく生成されたキーペアの詳細を確認できます。 「**[!UICONTROL 次へ]**」を選択して続行します。

![](../images/notifications/keypair-generated.png)

次の画面で、[!UICONTROL イベント登録の詳細]セクションにイベント登録の名前と説明を入力します。 ベストプラクティスは、同じプロジェクト内の他のユーザーとこのイベントの登録を区別できるように、一意で、簡単に識別できる名前を作成することです。

![](../images/notifications/registration-details.png)

[!UICONTROL イベントの受け取り方]セクションの下の同じ画面で、必要に応じてイベントの受け取り方を設定できます。 **** Webhookでは、イベントを受け取るカスタムWebフックアドレスを指定できますが、 **[!UICONTROL Runtime]** actionでは、 [Adobe I/O Runtimeを使用して同じことを行えます](https://www.adobe.io/apis/experienceplatform/runtime/docs.html)。

このチュートリアルでは、**[!UICONTROL Webhook]**&#x200B;を選択し、前に作成したWebフックのURLを指定します。 完了したら、「**[!UICONTROL 設定済みのイベントを保存]**」を選択してイベントの登録を完了します。

![](../images/notifications/receive-events.png)

新しく作成されたイベント登録の詳細ページが表示されます。このページでは、設定の編集、受信したイベントの確認、デバッグトレースの実行、新しいイベントプロバイダーの追加を行うことができます。

![](../images/notifications/registration-complete.png)

## 次の手順

このチュートリアルに従って、[!DNL Experience Platform]や[!DNL Privacy Service]の[!DNL I/O Event]通知を受信するWebフックを登録しました。 使用可能なイベントーと各サービスの通知ペイロードの解釈方法について詳しくは、次のドキュメントを参照してください。

* [[!DNL Privacy Service] 通知](../../privacy-service/privacy-events.md)
* [[!DNL Data Ingestion] 通知](../../ingestion/quality/subscribe-events.md)
* [[!DNL Flow Service (sources)] 通知](../../sources/notifications.md)

[!DNL Experience Platform]と[!DNL Privacy Service]でアクティビティを監視する方法について詳しくは、[[!DNL Observability Insights] 概要](../home.md)を参照してください。
