---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIを使用したストリーミング接続の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 181719e729748adcde62199c9406a97b7a807182

---


# UIを使用したストリーミング接続の作成

このUIガイドは、Adobe Experience Platformを使用したストリーミング接続の作成に役立ちます。

## はじめに

Experience Platformに開始ストリーミングデータを送信するには、まずストリーミングHTTP接続を作成する必要があります。 ストリーミング接続を作成する場合、ストリーミングデータのソース、および信頼できる（認証済み）ソースと信頼できない（認証済み）ソースのどちらからデータを送信するかなど、主要な詳細を指定する必要があります。

ストリーミング接続を登録すると、一意のURLが作成され、このURLを使用してプラットフォームにデータをストリーミングできます。

このガイドを完了するには、Adobe Experience Platformにアクセスする必要があります。 プラットフォームにアクセスできない場合は、先に進む前にシステム管理者に問い合わせてください。

## ストリーミング接続の作成

Experience Platform UIにログインした後、「 **Sources** 」をクリックして「 *Catalog* 」タブを開きます。 このページには、使用可能なソースタイプが個々のカードとして表示され、各カードには、ストリーミング接続からデータセットへの接続で作成されたデータフローの数を示すバブルが表示されます。

![](../images/streaming-ingestion/ui/click-sources.png)

ソースペ *ージで* 、「 **HTTP API**」をクリックし、「 **Connect source**」をクリックします。

![](../images/streaming-ingestion/ui/click-connect-source.png)

「HTTP *に接続* 」画面が表示されます。 「 *Service details*」で、新しいストリ **ーミング接** 続の名前と **** 説明を入力します。

「アカウ *ント認証*」で、ストリーミング接続の次の設定プロパティを選択します。

- **認証：** ストリーミング接続に認証が必要かどうか。 認証を行うと、信頼できるソースからデータが収集されます。 個人を特定できる情報(PII)を扱う場合は、このオプションをオンにすることをお勧めします。
- **XDMスキーマの互換性：** このストリーミング接続がXDM接続と互換性のあるイベントを送信するかどうかをスキーマします。 デフォルトでは、このプロパティはオンになっ **ていま**&#x200B;す。

設定プロパティの選択が完了したら、「 **Connect**」をクリックします。 これで、ストリーミングHTTP接続が作成され、ソースワークスペースの「参照 *」タブで表示できるようにな* りました ** 。

![](../images/streaming-ingestion/ui/http-sources-details.png)

「参照」タ *ブで* 、新しく作成したストリーミングHTTP接続をクリックし、その接続の詳細を表示できます。

![](../images/streaming-ingestion/ui/browse-sources.png)

接続名のハイパーリンクをクリックすると、「データを選択」をクリックして、接続するデータセットを設定することで、表示するデータを選 *択できます*。

![](../images/streaming-ingestion/ui/select-data.png)

新しいデータセッ [トを作成するか](#create-a-new-dataset) 、既存の [データセットを使用できます](#use-an-existing-dataset)。

### 新しいデータセットの作成

新しいデータセットを作成するには、 **Name**、 **Description**、およびデータセットの **ターゲット** スキーマを指定します。

![](../images/streaming-ingestion/ui/create-new-dataset.png)

すべての詳細を挿入し、「 **Next**」をクリックすると、「 **Finish** 」をクリックしてデータセットをストリーミングHTTP接続に接続する前に、提供された詳細を確認できます。

![](../images/streaming-ingestion/ui/review-create-new-dataset.png)

### 既存のデータセットの使用

既存のデータセットを使用するには、 **Outputデータセット名を選択します**。

![](../images/streaming-ingestion/ui/use-existing-dataset.png)

「 **Next**」をクリックした後、「 **Finish** 」をクリックして、選択したデータセットをストリーミングHTTP接続に接続する前に詳細を確認できます。

![](../images/streaming-ingestion/ui/review-existing-dataset.png)

## 次の手順

このチュートリアルに従って、ストリーミングHTTP接続を作成し、ストリーミングエンドポイントを使用して様々なデータ取り込みAPIにアクセスできるようにしました。 APIでストリーミング接続を作成する手順については、ストリーミング接続の作成のチュート [リアルを参照してください](../tutorials/create-streaming-connection.md)。
