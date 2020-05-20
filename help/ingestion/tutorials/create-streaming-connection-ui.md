---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIを使用したストリーミング接続の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 181719e729748adcde62199c9406a97b7a807182
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 0%

---


# UIを使用したストリーミング接続の作成

このUIガイドは、Adobe Experience Platformを使用してストリーミング接続を作成する際に役立ちます。

## はじめに

Experience Platformに開始ストリーミングデータを接続するには、まずストリーミングHTTP接続を作成する必要があります。 ストリーミング接続を作成する場合は、ストリーミングデータのソース、信頼できる（認証済み）ソースと信頼できない（未認証）ソースのどちらからデータを送信するかなど、主要な詳細情報を指定する必要があります。

ストリーミング接続を登録すると、プラットフォームにデータをストリーミングするのに使用できる一意のURLが生成されます。

このガイドを完了するには、Adobe Experience Platformにアクセスする必要があります。 プラットフォームにアクセスできない場合は、先に進む前に、システム管理者に問い合わせてください。

## ストリーミング接続の作成

Experience Platform UIにログインした後、「 **Sources** 」をクリックして「 *Catalog* 」タブを開きます。 このページには、使用可能なソースタイプが個別のカードとして表示されます。各カードには、ストリーミング接続からデータセットへの接続で作成されたデータフローの数を示すバブルが表示されます。

![](../images/streaming-ingestion/ui/click-sources.png)

「 *ソース* 」ページで、「 **HTTP API**」をクリックし、「 **ソースに**&#x200B;接続」をクリックします。

![](../images/streaming-ingestion/ui/click-connect-source.png)

「HTTP *に接続* 」画面が表示されます。 「 *Service details*」で、新しいストリーミング接続の **名前** と **** 説明の両方を入力します。

「 *アカウント認証*」で、ストリーミング接続用の次の設定プロパティを選択します。

- **認証：** ストリーミング接続に認証が必要かどうか。 認証により、信頼できるソースからデータが確実に収集されます。 個人識別情報(PII)を扱う場合は、このオプションをオンにすることをお勧めします。
- **XDMスキーマの互換性：** このストリーミングスキーマが、XDM接続と互換性のあるイベントを送信するかどうかを指定します。 デフォルトでは、このプロパティはオン **になっています**。

設定プロパティの選択が完了したら、「 **接続**」をクリックします。 これで、ストリーミングHTTP接続が作成され、 *ソース* ワークスペースの「 *参照* 」タブで表示できるようになります。

![](../images/streaming-ingestion/ui/http-sources-details.png)

「 *参照* 」タブから、新しく作成したストリーミングHTTP接続をクリックし、その接続の詳細を表示できます。

![](../images/streaming-ingestion/ui/browse-sources.png)

接続名のハイパーリンクをクリックすると、「データを *選択*」をクリックして、接続するデータセットを設定することで、表示するデータを選択できます。

![](../images/streaming-ingestion/ui/select-data.png)

新しいデータセットを [作成することも](#create-a-new-dataset) 、既存のデータセットを [使用することもできます](#use-an-existing-dataset)。

### 新しいデータセットの作成

新しいデータセットを作成するには、 **名前**、 **説明**、およびデータセットのターゲット **** スキーマを指定します。

![](../images/streaming-ingestion/ui/create-new-dataset.png)

すべての詳細を挿入して「 **次へ**」をクリックすると、「 **完了** 」をクリックしてデータセットをストリーミングHTTP接続に接続する前に、提供された詳細を確認できます。

![](../images/streaming-ingestion/ui/review-create-new-dataset.png)

### 既存のデータセットの使用

既存のデータセットを使用するには、 **Outputデータセット名を選択します**。

![](../images/streaming-ingestion/ui/use-existing-dataset.png)

「 **次へ**」をクリックした後で **、「** 完了」をクリックする前に詳細を確認し、選択したデータセットをストリーミングHTTP接続に接続します。

![](../images/streaming-ingestion/ui/review-existing-dataset.png)

## 次の手順

このチュートリアルに従って、ストリーミングHTTP接続を作成し、ストリーミングエンドポイントを使用して様々なデータ取り込みAPIにアクセスできるようにします。 APIでストリーミング接続を作成する手順については、ストリーミング接続の [作成のチュートリアルを参照してください](../tutorials/create-streaming-connection.md)。
