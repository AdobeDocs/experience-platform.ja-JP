---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのクラウドストレージストリーミングコネクタのデータフローの設定
topic: overview
translation-type: tm+mt
source-git-commit: 168ac3a3ab9f475cb26dc8138cbc90a3e35c836d
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 12%

---


# UIでのクラウドストレージストリーミングコネクタのデータフローの設定

データフローとは、ソースからデータセットにデータを取得し、取り込むスケジュール済みのタスク [!DNL Platform] です。 このチュートリアルでは、クラウドストレージベースのコネクタを使用して新しいデータフローを設定する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

また、このチュートリアルでは、クラウドストレージコネクタを既に作成済みである必要があります。 UIで異なるクラウドストレージコネクタを作成するためのチュートリアルのリストは、 [ソースコネクタの概要](../../../../home.md)。

## データの選択

クラウドストレージコネクタを作成すると、 *データの選択* 手順が表示され、データのストリーミング元となるストリームを選択するためのインターフェイスが提供されます。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-data.png)

## データフィールドのXDMスキーマへのマッピング

「 *マッピング* 」の手順が表示され、ソースデータをデータセットにマッピングするインタラクティブなインターフェイスが提供され [!DNL Platform] ます。

取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

**既存のデータセットを使用する**

既存のデータセットにデータを取り込むには、「 **[!UICONTROL Use existing dataset]**」を選択し、データセットアイコンをクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/use-existing-data.png)

The _Select dataset_ dialog appears. 使用するデータセットを見つけて選択し、「 **[!UICONTROL 続行]**」をクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-existing-data.png)

**新しいデータセットの使用**

データを新しいデータセットに取り込むには、「 **[!UICONTROL 新しいデータセットを]** 作成」を選択し、表示されるフィールドにデータセットの名前と説明を入力します。 次に、使用するスキーマをドロップダウンで選択します。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/use-new-dataset.png)

## データフローに名前を付ける

[ *[!UICONTROL Dataflow detail]* ]ステップが表示され、新しいデータフローに名前を付け、簡単に説明を付けることができます。

データフローの値を指定し、「 **[!UICONTROL 次へ]**」をクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/name-your-dataflow.png)

### データフローの確認

「 *レビュー* 」ステップが表示され、新しいデータフローを作成前に確認できます。 詳細は次のカテゴリに分類されます。

- *[!UICONTROL ソースの詳細]*: ソースのタイプおよびソースに関するその他の関連詳細を表示します。
- *[!UICONTROL Targetの詳細]*: ソースデータが取り込まれるデータセット(データセットに従うスキーマなど)を示します。

データフローをレビューしたら、 **[!UICONTROL 「Finish]** 」をクリックし、データフローを作成するまでの時間を設定します。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/review.png)

## データフローの監視と削除

クラウドストレージのデータフローが作成されたら、データを通じて取り込まれるデータを監視できます。 データ・フローの監視および削除の詳細は、「データ・フローの [監視に関するチュートリアル](../../../../../ingestion/quality/monitor-data-flows.md)」を参照してください。

## 次の手順

このチュートリアルに従うと、外部のクラウドストレージからデータを取り込むためのデータフローが正しく作成され、データセットの監視に関する洞察が得られます。 受信データは、やなどのダウンストリーム [!DNL Platform] サービスで使用でき [!DNL Real-time Customer Profile] るようになり [!DNL Data Science Workspace]ました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
- [Data Science ワークスペースの概要](../../../../../data-science-workspace/home.md)

## 付録

以下の節では、ソースコネクタを使用する場合の追加情報について説明します。

### データフローの無効化

データフローが作成されると、そのデータはすぐにアクティブになり、指定されたスケジュールに従ってデータを取り込みます。 アクティブなデータフローは、次の手順に従っていつでも無効にできます。

「 *[!UICONTROL ソース]* 」ワークスペース内で、「 **[!UICONTROL 参照]** 」タブをクリックします。 次に、無効にするアクティブなデータフローに関連付けられているベース接続の名前をクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/browse.png)

「 *[!UICONTROL ソースアクティビティ]* 」ページが表示されます。 リストからアクティブなデータフローを選択し、画面の右側に *「Properties* 」列を開きます。この列には「 **[!UICONTROL Enabled]** 」トグル・ボタンが含まれています。 切り替えボタンをクリックして、データフローを無効にします。 同じ切り替えを使用して、データフローを無効にした後で再び有効にできます。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/disable-source.png)

### 母集団の受信データを有効にし [!DNL Profile] ます

ソースコネクタから受信するデータは、データの富化と埋め込みに使用でき [!DNL Real-time Customer Profile] ます。 データの入力について詳しくは、 [!DNL Real-time Customer Profile][プロファイルの入力に関するチュートリアルを参照してください](../../profile.md)。
