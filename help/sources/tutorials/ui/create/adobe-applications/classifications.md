---
keywords: Experience Platform；ホーム；人気の高いトピック；分析；分類
description: UI でAdobe Analyticsソースコネクタを作成して、分類データをAdobe Experience Platformに取り込む方法を説明します。
solution: Experience Platform
title: UI での分類データ用のAdobe Analyticsソース接続の作成
type: Tutorial
exl-id: d606720d-f1ca-47cc-919b-643a8fc61e07
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 25%

---

# UI での分類データ用のAdobe Analyticsソース接続の作成

このチュートリアルでは、UI でAdobe Analytics Classifications データソース接続を作成して、分類データをAdobe Experience Platformに取り込む手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md):Experience Platformは、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

Analytics Classifications Data Connector では、新しい [!DNL Classifications] 使用前のAdobe Analyticsのインフラストラクチャ。 データの移行ステータスを確認するには、担当のAdobeカスタマーサクセスマネージャーにお問い合わせください。

## 分類を選択

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから、ソースワークスペースにアクセスします。 この **[!UICONTROL カタログ]** 画面には、インバウンド接続を作成するために使用可能なソースが表示されます。 各ソースカードには、新しいアカウントを設定するか、既存のアカウントにデータを追加するかのどちらかのオプションが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL Adobe]** カテゴリの **[!UICONTROL Adobe Analytics]** カードを選択し、 **[!UICONTROL データを追加]** をクリックして、Analytics 分類データの操作を開始します。

![](../../../../images/tutorials/create/classifications/catalog.png)

**[!UICONTROL Analytics ソースデータの追加]**&#x200B;の手順が表示されます。選択 **[!UICONTROL 分類]** 上部のヘッダーから [!DNL Classifications] データセット。ディメンション ID、レポートスイート名、レポートスイート ID に関する情報を含みます。

各ページは最大 10 個の異なる [!DNL Classifications] 選択できるデータセット。 選択 **[!UICONTROL 次へ]** をクリックして、その他のオプションを参照します。 右側のパネルには、 [!DNL Classifications] 選択したデータセットとその名前。 また、このパネルでは、 [!DNL Classifications] 誤って選択したデータセット、または 1 つのアクションですべての選択を解除した可能性があります。

最大 30 個の異なる [!DNL Classifications] に取り込むデータセット [!DNL Platform].

次を選択したら、 [!DNL Classifications] データセット、選択 **[!UICONTROL 次へ]** をクリックします。

![](../../../../images/tutorials/create/classifications/add-data.png)

## 分類のレビュー

この **[!UICONTROL レビュー]** ステップが表示され、選択した [!DNL Classifications] データセットを作成する前に設定します。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースプラットフォームと接続のステータスを表示します。
* **[!UICONTROL データタイプ]**:選択した数を表示 [!DNL Classifications].
* **[!UICONTROL スケジュール]**:同期の頻度を表示します [!DNL Classifications] データ。

データフローをレビューしたら、「 」をクリックします。 **[!UICONTROL 完了]** とは、データフローが作成されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/classifications/review.png)

## 分類データフローの監視

データフローを作成したら、それを通じて取り込まれるデータを監視できます。 **[!UICONTROL カタログ]**&#x200B;画面から「**[!UICONTROL データフロー]**」を選択すると、 アカウントに関連する確立済みフローのリストが表示されます。[!DNL Classifications]

![](../../../../images/tutorials/create/classifications/dataflows.png)

**[!UICONTROL データフロー]**&#x200B;画面が表示されます。 このページには、データフローのリストがあり、その名前、ソースデータ、データフローの実行ステータスに関する情報が含まれます。 右側にはが表示されます。 **[!UICONTROL プロパティ]** 次に関するメタデータを含むパネル： [!DNL Classifications] データフロー。

を選択します。 **[!UICONTROL Target データセット]** にアクセスします。

![](../../../../images/tutorials/create/classifications/list-of-dataflows.png)

この **[!UICONTROL データセットアクティビティ]** ページには、選択したターゲットデータセットに関する情報（バッチステータス、データセット ID、スキーマに関する詳細など）が表示されます。

>[!IMPORTANT]
>
>データセットの削除は、他のソースコネクタでは可能ですが、Analytics 分類データコネクタでは現在サポートされていません。データセットを誤って削除した場合は、アドビカスタマーサポートにお問い合わせください。

![](../../../../images/tutorials/create/classifications/dataset.png)


## 次の手順

このチュートリアルに従って、次の機能を引き出す Analytics 分類データコネクタを作成しました [!DNL Classifications] データを [!DNL Platform]. 詳しくは、次のドキュメントを参照してください。 [!DNL Analytics] および [!DNL Classifications] データ：

* [Analytics Data コネクタの概要](../../../../connectors/adobe-applications/analytics.md)
* [UI での Analytics データ接続の作成](./analytics.md)
* [分類について](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=ja)
