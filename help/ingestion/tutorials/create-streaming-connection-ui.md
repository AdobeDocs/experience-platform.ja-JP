---
keywords: Experience Platform；ホーム；人気のあるトピック；ストリーミング接続；ストリーミング接続の作成；uiガイド；チュートリアル；ストリーミング接続の作成；ストリーミング取り込み；取り込み；
solution: Experience Platform
title: UI を使用したストリーミング接続の作成
topic: tutorial
type: Tutorial
description: この UI ガイドは、Adobe Experience Platform を使用してストリーミング接続を作成する際に役立ちます。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 71%

---


# UI を使用したストリーミング接続の作成

この UI ガイドは、Adobe Experience Platform を使用してストリーミング接続を作成する際に役立ちます。

## はじめに

[!DNL Experience Platform]に開始ストリーミングデータを接続するには、まずストリーミングHTTP接続を作成する必要があります。 ストリーミング接続を作成する場合、ストリーミングデータのソースや、信頼できる（認証済み）ソースまたは信頼できない（未認証）ソースからデータを送信するかどうかなど、重要な設定をおこなう必要があります。

ストリーミング接続を登録すると、[!DNL Platform]にデータをストリーミングするのに使用できる一意のURLが生成されます。

このガイドに記載されている操作を完了するには、Adobe Experience Platform にアクセスする必要があります。[!DNL Platform]へのアクセス権がない場合は、先に進む前にシステム管理者に問い合わせてください。

## ストリーミング接続の作成

[!DNL Experience Platform] UIにログインした後、**[!UICONTROL ソース]**&#x200B;をクリックして、「**[!UICONTROL カタログ]**」タブを開きます。 このページには、使用可能なソースの種類が個々のカードで表示されます。各カードの丸の中に、データセットへのストリーミング接続から作成されたデータフローの数が示されます。

![](../images/streaming-ingestion/ui/click-sources.png)

「**[!UICONTROL ソース]**」ページで、「**[!UICONTROL HTTP API]**」をクリックしてから、「**[!UICONTROL 接続ソース]**」をクリックします。

![](../images/streaming-ingestion/ui/click-connect-source.png)

「**[!UICONTROL HTTP に接続]**」画面が表示されます。「**[!UICONTROL サービスの詳細]**」で、新しいストリーミング接続の名前と説明を入力します。

「**[!UICONTROL アカウント認証]**」で、ストリーミング接続の次の設定プロパティを選択します。

- **[!UICONTROL 認証]:** ストリーミング接続に認証が必要かどうか。認証を実行すると、データは信頼できるソースから収集されます。個人を特定できる情報（Personally Identifiable Information：PII）を扱う場合は、このオプションをオンにすることをお勧めします。
- **[!UICONTROL XDMスキーマの互換性]:** このストリーミングスキーマが、XDM接続と互換性のあるイベントを送信するかどうかを指定します。デフォルトでは、このプロパティは&#x200B;**オン**&#x200B;になっています。

設定プロパティの選択が完了したら、「**[!UICONTROL 接続]**」をクリックします。これで、ストリーミング HTTP 接続が作成され、「**[!UICONTROL ソース]**」ワークスペースの「**[!UICONTROL 参照]**」タブに表示されるようになりました。

![](../images/streaming-ingestion/ui/http-sources-details.png)

「**[!UICONTROL 参照]**」タブで、新しく作成したストリーミング HTTP 接続をクリックすると、その接続の詳細が表示されます。

![](../images/streaming-ingestion/ui/browse-sources.png)

接続名のハイパーリンクをクリックして「**[!UICONTROL データを選択]**」をクリックすると、接続するデータセットを設定して、表示するデータを選択できます。

![](../images/streaming-ingestion/ui/select-data.png)

[新しいデータセットを作成する](#create-a-new-dataset)か、[既存のデータセットを使用](#use-an-existing-dataset)することができます。

### 新しいデータセットを作成する

新しいデータセットを作成するには、名前、説明、およびターゲットセットのスキーマを指定します。

![](../images/streaming-ingestion/ui/create-new-dataset.png)

すべての情報を入力して「**[!UICONTROL 次へ]**」をクリックすると、指定した情報を確認してから、「**[!UICONTROL 終了]**」をクリックしてデータセットをストリーミング HTTP 接続に接続できます。

![](../images/streaming-ingestion/ui/review-create-new-dataset.png)

### 既存のデータセットを使用する

既存のデータセットを使用するには、「**[!UICONTROL 出力データセット名]**」を選択します。

![](../images/streaming-ingestion/ui/use-existing-dataset.png)

「**[!UICONTROL 次へ]**」をクリックして詳細を確認してから、「**[!UICONTROL 終了]**」をクリックして、選択したデータセットをストリーミング HTTP 接続に接続できます。

![](../images/streaming-ingestion/ui/review-existing-dataset.png)

## 次の手順

このチュートリアルに従って、ストリーミングHTTP接続を作成し、ストリーミングエンドポイントを使用して様々な[!DNL Data Ingestion] APIにアクセスできるようにします。 API でストリーミング接続を作成する手順については、[ストリーミング接続の作成に関するチュートリアル](../tutorials/create-streaming-connection.md)を参照してください。
