---
keywords: Experience Platform;home;popular topics;streaming connection;create streaming connection;ui guide;tutorial;create a streaming connection;streaming ingestion;ingestion;
solution: Experience Platform
title: UI を使用したストリーミング接続の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: d2f098cb9e4aaf5beaad02173a22a25a87a43756
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 77%

---


# UI を使用したストリーミング接続の作成

この UI ガイドは、Adobe Experience Platform を使用してストリーミング接続を作成する際に役立ちます。

## はじめに

In order to start streaming data to [!DNL Experience Platform], you must first create a streaming HTTP connection. ストリーミング接続を作成する場合、ストリーミングデータのソースや、信頼できる（認証済み）ソースまたは信頼できない（未認証）ソースからデータを送信するかどうかなど、重要な設定をおこなう必要があります。

After registering a streaming connection you will have a unique URL which can be used to stream data to [!DNL Platform].

このガイドに記載されている操作を完了するには、Adobe Experience Platform にアクセスする必要があります。If you do not have access to [!DNL Platform], please contact your system administrator before proceeding.

## ストリーミング接続の作成

After logging in to the [!DNL Experience Platform] UI, click **[!UICONTROL Sources]** to open the **[!UICONTROL Catalog]** tab. このページには、使用可能なソースの種類が個々のカードで表示されます。各カードの丸の中に、データセットへのストリーミング接続から作成されたデータフローの数が示されます。

![](../images/streaming-ingestion/ui/click-sources.png)

「**[!UICONTROL ソース]**」ページで、「**[!UICONTROL HTTP API]**」をクリックしてから、「**[!UICONTROL 接続ソース]**」をクリックします。

![](../images/streaming-ingestion/ui/click-connect-source.png)

「**[!UICONTROL HTTP に接続]**」画面が表示されます。「**[!UICONTROL サービスの詳細]**」で、新しいストリーミング接続の&#x200B;**[!UICONTROL 名前]**&#x200B;と&#x200B;**[!UICONTROL 説明]**&#x200B;を入力します。

「**[!UICONTROL アカウント認証]**」で、ストリーミング接続の次の設定プロパティを選択します。

- **[!UICONTROL 認証]:** ストリーミング接続に認証が必要かどうか。 認証を実行すると、データは信頼できるソースから収集されます。個人を特定できる情報（Personally Identifiable Information：PII）を扱う場合は、このオプションをオンにすることをお勧めします。
- **[!UICONTROL XDMスキーマの互換性]:** このストリーミングスキーマが、XDM接続と互換性のあるイベントを送信するかどうかを指定します。 デフォルトでは、このプロパティは&#x200B;**オン**&#x200B;になっています。

設定プロパティの選択が完了したら、「**[!UICONTROL 接続]**」をクリックします。これで、ストリーミング HTTP 接続が作成され、「**[!UICONTROL ソース]**」ワークスペースの「**[!UICONTROL 参照]**」タブに表示されるようになりました。

![](../images/streaming-ingestion/ui/http-sources-details.png)

「**[!UICONTROL 参照]**」タブで、新しく作成したストリーミング HTTP 接続をクリックすると、その接続の詳細が表示されます。

![](../images/streaming-ingestion/ui/browse-sources.png)

接続名のハイパーリンクをクリックして「**[!UICONTROL データを選択]**」をクリックすると、接続するデータセットを設定して、表示するデータを選択できます。

![](../images/streaming-ingestion/ui/select-data.png)

[新しいデータセットを作成する](#create-a-new-dataset)か、[既存のデータセットを使用](#use-an-existing-dataset)することができます。

### 新しいデータセットを作成する

新しいデータセットを作成するには、**[!UICONTROL 名前]**、**[!UICONTROL 説明]**&#x200B;およびデータセットのターゲット&#x200B;**[!UICONTROL スキーマ]**&#x200B;を指定します。

![](../images/streaming-ingestion/ui/create-new-dataset.png)

すべての情報を入力して「**[!UICONTROL 次へ]**」をクリックすると、指定した情報を確認してから、「**[!UICONTROL 終了]**」をクリックしてデータセットをストリーミング HTTP 接続に接続できます。

![](../images/streaming-ingestion/ui/review-create-new-dataset.png)

### 既存のデータセットを使用する

既存のデータセットを使用するには、「**[!UICONTROL 出力データセット名]**」を選択します。

![](../images/streaming-ingestion/ui/use-existing-dataset.png)

「**[!UICONTROL 次へ]**」をクリックして詳細を確認してから、「**[!UICONTROL 終了]**」をクリックして、選択したデータセットをストリーミング HTTP 接続に接続できます。

![](../images/streaming-ingestion/ui/review-existing-dataset.png)

## 次の手順

By following this tutorial, you have created a streaming HTTP connection, enabling you to use the streaming endpoint to access a variety of [!DNL Data Ingestion] APIs. API でストリーミング接続を作成する手順については、[ストリーミング接続の作成に関するチュートリアル](../tutorials/create-streaming-connection.md)を参照してください。
