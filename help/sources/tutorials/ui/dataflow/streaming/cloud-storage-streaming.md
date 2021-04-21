---
keywords: Experience Platform；ホーム；人気の高いトピック；クラウドストレージコネクタ；クラウドストレージ
solution: Experience Platform
title: UIでのクラウドストレージストリーミングコネクタのデータフローの設定
topic-legacy: overview
type: Tutorial
description: データフローとは、ソースからプラットフォームデータセットにデータを取得し、取り込むスケジュール設定されたタスクです。 このチュートリアルでは、クラウドストレージベースのコネクタを使用して新しいデータフローを設定する手順を説明します。
exl-id: 75deead6-ef3c-48be-aed2-c43d1f432178
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '705'
ht-degree: 6%

---

# UIでのクラウドストレージストリーミング接続のデータフローの設定

データフローとは、ソースからデータセット[!DNL Platform]にデータを取得し、取り込むスケジュール済みのタスクです。 このチュートリアルでは、クラウドストレージベースのコネクタを使用して新しいデータフローを設定する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

また、このチュートリアルでは、クラウドストレージコネクタを既に作成済みである必要があります。 UIで異なるクラウドストレージコネクタを作成するためのチュートリアルのリストは、[source connectors overview](../../../../home.md)にあります。

## データの選択

クラウドストレージコネクタを作成すると、*データを選択*&#x200B;の手順が表示され、データのストリーミング元となるストリームを選択するためのインターフェイスが提供されます。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-data.png)

## データフィールドのXDMスキーマへのマッピング

**[!UICONTROL マッピング]**&#x200B;の手順が表示され、ソースデータを[!DNL Platform]データセットにマッピングするためのインタラクティブなインターフェイスが提供されます。

取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

**既存のデータセットを使用する**

既存のデータセットにデータを取り込むには、**[!UICONTROL 既存のデータセット]**&#x200B;を使用を選択し、データセットアイコンをクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/use-existing-data.png)

**[!UICONTROL データセットを選択]**&#x200B;ダイアログが表示されます。 使用するデータセットを見つけて選択し、**[!UICONTROL 続行]**&#x200B;をクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-existing-data.png)

**新しいデータセットの使用**

データを新しいデータセットに取り込むには、**[!UICONTROL 新しいデータセットを作成]**&#x200B;を選択し、提供されたフィールドにデータセットの名前と説明を入力します。 次に、使用するスキーマをドロップダウンで選択します。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/use-new-dataset.png)

## データフローに名前を付ける

**[!UICONTROL Dataflow detail]**&#x200B;ステップが表示され、新しいデータフローに名前を付け、簡単な説明を入力できます。

データフローの値を指定し、**[!UICONTROL 次へ]**&#x200B;をクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/name-your-dataflow.png)

### データフローの確認

「**[!UICONTROL レビュー]**」ステップが表示され、新しいデータフローを作成前に確認できます。 詳細は次のカテゴリに分類されます。

- **[!UICONTROL ソースの詳細]**:ソースのタイプおよびソースに関するその他の関連詳細を表示します。
- **[!UICONTROL ターゲットの詳細]**:ソースデータが取り込まれるデータセット(データセットに従うスキーマなど)を示します。

データフローを確認したら、**[!UICONTROL 「Finish]**」をクリックし、データフローの作成に時間を割り当てます。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/review.png)

## データフローの監視と削除

クラウドストレージのデータフローが作成されたら、データを通じて取り込まれるデータを監視できます。 データフローの監視と削除の詳細については、[データフローの監視](../../../../../ingestion/quality/monitor-data-ingestion.md)のチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、外部のクラウドストレージからデータを取り込むためのデータフローが正しく作成され、データセットの監視に関する洞察が得られます。 受信データは、[!DNL Real-time Customer Profile]や[!DNL Data Science Workspace]などのダウンストリーム[!DNL Platform]サービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [[!DNL Real-time Customer Profile] 概要](../../../../../profile/home.md)
- [[!DNL Data Science Workspace] 概要](../../../../../data-science-workspace/home.md)

## 付録

以下の節では、ソースコネクタを使用する場合の追加情報について説明します。

### データフローの無効化

データフローが作成されると、そのデータはすぐにアクティブになり、指定されたスケジュールに従ってデータを取り込みます。 アクティブなデータフローは、次の手順に従っていつでも無効にできます。

**[!UICONTROL ソース]**&#x200B;ワークスペース内で、**[!UICONTROL 「参照]**」タブをクリックします。 次に、無効にするアクティブなデータフローに関連付けられている接続の名前をクリックします。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/browse.png)

**[!UICONTROL ソースアクティビティ]**&#x200B;ページが表示されます。 リストからアクティブなデータフローを選択し、画面の右側に&#x200B;**[!UICONTROL Properties]**&#x200B;列を開きます。この列には&#x200B;**[!UICONTROL Enabled]**&#x200B;トグルボタンが含まれています。 切り替えボタンをクリックして、データフローを無効にします。 同じ切り替えを使用して、データフローを無効にした後で再び有効にできます。

![](../../../../images/tutorials/dataflow/cloud-storage/streaming/disable-source.png)

### [!DNL Profile]母集団の受信データをアクティブ化

ソースコネクタからの受信データは、[!DNL Real-time Customer Profile]データを豊かにし、埋め込むために使用できます。 [!DNL Real-time Customer Profile]データの入力について詳しくは、[プロファイルの母集団](../../profile.md)のチュートリアルを参照してください。
