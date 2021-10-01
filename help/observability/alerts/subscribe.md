---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
title: Adobe I/O イベント通知の登録
description: このドキュメントでは、Adobe Experience Platform サービスの Adobe I/O イベント通知を登録する手順を説明します。利用可能なイベントタイプに関する参照情報も、該当する各 [!DNL Platform] サービスに対して返されたイベントデータを解釈する方法に関する詳細なドキュメントへのリンクとともに提供されます。
feature: Alerts
exl-id: c0ad7217-ce84-47b0-abf6-76bcf280f026
source-git-commit: d82487f34c0879ed27ac55e42d70346f45806131
workflow-type: ht
source-wordcount: '773'
ht-degree: 100%

---

# Adobe I/O イベント通知の登録

[!DNL Observability Insights] により、Adobe Experience Platform アクティビティに関する Adobe I/O イベント通知を登録できます。これらのイベントは、アクティビティ監視の効率的な自動化を促進するために、設定済みの Webhook に送信されます。

このドキュメントでは、Adobe Experience Platform サービスの Adobe I/O イベント通知を登録する手順を説明します。利用可能なイベントタイプに関する参照情報も、該当する各[!DNL Platform]サービスに対して返されたイベントデータを解釈する方法に関する詳細なドキュメントへのリンクとともに提供されます。

## はじめに

このドキュメントでは、Webhook と、あるアプリケーションから別のアプリケーションに Webhook を接続する方法についての実践的な理解が必要です。Webhook の概要については、[[!DNL I/O Events] ドキュメント](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)を参照してください。

## Webhook の作成

[!DNL I/O Event] 通知を受け取るには、イベント登録の詳細の一部として一意の Webhook URL を指定して Webhook を登録する必要があります。

選択したクライアントを使用して、Webhook を設定できます。このチュートリアルの一部として使用する一時的な Webhook アドレスについては、[Webhook.site](https://webhook.site/) にアクセスし、提供された一意の URL をコピーします。

![](../images/notifications/webhook-url.png)

最初の検証プロセス中に、[!DNL I/O Events] は GET リクエストで `challenge` のクエリパラメーターを Webhook に送信します。このパラメーターの値を応答ペイロードで返すように Webhook を設定する必要があります。Webhook.site を使用している場合は、右上隅の **[!DNL Edit]** を選択し、**[!DNL Response body]** の下に「`$request.query.challenge$`」と入力してから「**[!DNL Save]**」を選択します。

![](../images/notifications/response-challenge.png)

## Adobe Developer Console で新規プロジェクトを作成します

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Adobe Developer Console のドキュメントの[空のプロジェクトの作成](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)チュートリアルで概説されている手順に従います。

## イベントの登録

新しいプロジェクトを作成したら、そのプロジェクトの概要画面に移動します。 ここから、「**[!UICONTROL イベントの追加]**」を選択します。

![](../images/notifications/add-event-button.png)

プロジェクトにイベントプロバイダーを追加できるダイアログが表示されます。

* Experience Platform アラートを登録している場合は、「**[!UICONTROL Platform 通知]**」を選択します。
* Adobe Experience Platform の [!DNL Privacy Service] 通知を登録している場合は、「**[!UICONTROL Privacy Service イベント]**」を選択します。

イベントプロバイダーを選択したら、「**[!UICONTROL 次へ]**」を選択します。

![](../images/notifications/event-provider.png)

次の画面に、登録するイベントタイプのリストが表示されます。登録するイベントを選択し、「**[!UICONTROL 次へ]**」を選択します。

>[!NOTE]
>
>使用しているサービスでどのイベントに登録すればよいかわからない場合は、サービス別の通知に関するドキュメントを参照してください。
>
>* [[!DNL Privacy Service] 通知](../../privacy-service/privacy-events.md)
>* [[!DNL Data Ingestion] 通知](../../ingestion/quality/subscribe-events.md)
>* [[!DNL Flow Service (sources)] 通知](../../sources/notifications.md)


![](../images/notifications/choose-event-subscriptions.png)

次の画面では、JSON Webトークン（JWT）の作成を求められます。キーペアを自動的に生成するか、ターミナルで生成された独自の公開キーをアップロードするかを選択できます。

このチュートリアルでは、最初のオプションに従います。「**[!UICONTROL キーペアを生成]**」のオプションボックスを選択し、右下隅にある「**[!UICONTROL キーペアを生成]**」ボタンを選択します。

![](../images/notifications/generate-keypair.png)

生成されたキーペアは、ブラウザーによって自動的にダウンロードされます。 このファイルは、Developer Console 内には保持されないので、自分で保存する必要があります。

次の画面では、新しく生成されたキーペアの詳細を確認できます。 「**[!UICONTROL 次へ]**」をクリックして続行します。

![](../images/notifications/keypair-generated.png)

次の画面で、「[!UICONTROL イベント登録の詳細]」セクションにイベント登録の名前と説明を入力します。ベストプラクティスは、このイベント登録を同じプロジェクト内の他のイベントと区別できるように、識別しやすい一意の名前を作成することです。

![](../images/notifications/registration-details.png)

「[!UICONTROL イベントの受信方法]」セクションと同じ画面のさらに下で、オプションでイベントの受信方法を設定できます。**[!UICONTROL Webhook]** では、イベントを受け取るためのカスタム Webhook アドレスを提供できます。一方、**[!UICONTROL Runtime アクション]**&#x200B;では、[Adobe I/O Runtime](https://www.adobe.io/apis/experienceplatform/runtime/docs.html) を使用して同じ処理を実行できます。

このチュートリアルでは、「**[!UICONTROL Webhook]**」を選択し、前に作成した Webhook の URL を入力します。完了したら、「**[!UICONTROL 設定済みのイベントを保存]**」を選択してイベント登録を完了します。

![](../images/notifications/receive-events.png)

新しく作成されたイベント登録の詳細ページが表示され、設定の編集、受信したイベントの確認、デバッグトレースの実行、新しいイベントプロバイダーの追加をおこなうことができます。

![](../images/notifications/registration-complete.png)

## 次の手順

このチュートリアルに従い、[!DNL Experience Platform] および／または [!DNL Privacy Service] の [!DNL I/O Event] 通知を受け取る Webhook を登録しました。使用可能なイベントと各サービスの通知ペイロードの解釈方法について詳しくは、次のドキュメントを参照してください。

* [[!DNL Privacy Service] 通知](../../privacy-service/privacy-events.md)
* [[!DNL Data Ingestion] 通知](../../ingestion/quality/subscribe-events.md)
* [[!DNL Flow Service] （ソース）通知](../../sources/notifications.md)

[!DNL Experience Platform] と [!DNL Privacy Service] でアクティビティを監視する方法について詳しくは、[[!DNL Observability Insights] 概要](../home.md)を参照してください。
