---
keywords: Experience Platform;home;popular topics; analytics;classifications
description: このチュートリアルでは、UIで分類データをAdobe Experience Platformに取り込むためのAdobe Analytics分類データコネクタを作成する手順を説明します。
solution: Experience Platform
title: UIでのAdobe Analytics分類データコネクタの作成
topic: overview
type: Tutorial
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 4%

---


# UIでのAdobe Analytics分類データコネクタの作成

このチュートリアルでは、UIで分類データをAdobe Experience Platformに取り込むためのAdobe Analytics分類データコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md):Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Analytics Classifications Data Connectorは、使用前に、データをAdobe Analyticsの新しい [!DNL Classifications] インフラストラクチャに移行した必要があります。 データの移行ステータスを確認するには、Adobeのカスタマーサクセスマネージャーにお問い合わせください。

## 分類の選択

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) ソース **** 」を選択してソースワークスペースにアクセスします。 カ **[!UICONTROL タログ]** 画面には、受信接続を作成する際に使用できるソースが表示されます。 各ソースカードには、新しいアカウントを設定するか、既存のアカウントにデータを追加するかのオプションが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 **[!UICONTROL Adobeアプリ]** カテゴリ」で、 **[!UICONTROL Adobe Analytics]** カードを選択し、「Analytics分類データを使用する開始のデータ **** 」を選択します。

![](../../../../images/tutorials/create/classifications/catalog.png)

「 **[!UICONTROL Analyticsソースのデータを追加]** 」手順が表示されます。 上部のヘッダーで **[!UICONTROL 「]** 分類 [!DNL Classifications] 」を選択し、ディメンションID、レポートスイート名およびレポートスイートIDに関する情報など、データセットのリストを確認します。

各ページには、選択可能な10個の [!DNL Classifications] データセットが表示されます。 ページ下部の **[!UICONTROL 「次へ]** 」を選択して、他のオプションを参照します。 右側のパネルには、選択した [!DNL Classifications] データセットの総数と名前が表示されます。 また、このパネルでは、誤って選択した [!DNL Classifications] データセットをすべて削除したり、1回の操作ですべての選択を解除したりできます。

最大30種類の異なる [!DNL Classifications] データセットを選択してに取り込むことができ [!DNL Platform]ます。

データセットを選択したら、 [!DNL Classifications] ページの右上にある「 **[!UICONTROL 次へ]** 」を選択します。

![](../../../../images/tutorials/create/classifications/add-data.png)

## 分類の確認

「 **[!UICONTROL レビュー]** 」(Review [!DNL Classifications] )ステップが表示され、選択したデータセットを作成前に確認できます。 詳細は次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:接続元プラットフォームと接続の状態を表示します。
* **[!UICONTROL データタイプ]**:選択した数が表示され [!DNL Classifications]ます。
* **[!UICONTROL スケジュール]**:データの同期の頻度を示し [!DNL Classifications] ます。

データフローをレビューしたら、 **[!UICONTROL 「Finish]** 」をクリックし、データフローを作成するまでの時間を設定します。

![](../../../../images/tutorials/create/classifications/review.png)

## 分類のデータフローの監視

データフローを作成したら、データフローを介して取り込まれるデータを監視できます。 「 **[!UICONTROL カタログ]** 」画面で、「 **[!UICONTROL データフロー]**[!DNL Classifications] 」を選択して、アカウントに関連付けられた確立済みフローのリストを表示します。

![](../../../../images/tutorials/create/classifications/dataflows.png)

[ **[!UICONTROL データフロー]** ]画面が表示されます。 このページは、名前、ソース・データおよびデータ・フロー実行ステータスに関するリストを含むデータ・フローの情報です。 右側は、データフローに関するメタデータを含む **[!UICONTROL プロパティ]** ・パネルで [!DNL Classifications] す。

アクセスする **[!UICONTROL ターゲットデータセット]** を選択します。

![](../../../../images/tutorials/create/classifications/list-of-dataflows.png)

「 **[!UICONTROL データセットアクティビティ]** 」ページには、選択したターゲットデータセットに関する情報(バッチステータス、データセットID、スキーマなど)が表示されます。

>[!IMPORTANT]
>
>他のソースコネクターではデータセットの削除は可能ですが、現在、Analytics分類データコネクターではサポートされていません。 誤ってデータセットを削除した場合は、Adobeカスタマーケアにお問い合わせください。

![](../../../../images/tutorials/create/classifications/dataset.png)


## 次の手順

このチュートリアルに従って、にデータを導入するAnalytics分類データコネクタを作成し [!DNL Classifications] ま [!DNL Platform]した。 See the following documents for more information on [!DNL Analytics] and [!DNL Classifications] data:

* [Analytics Data Connectorの概要](../../../../connectors/adobe-applications/analytics.md)
* [UIでのAnalyticsデータコネクタの作成](./analytics.md)
* [分類について](https://docs.adobe.com/content/help/ja-JP/analytics/components/classifications/c-classifications.html#)