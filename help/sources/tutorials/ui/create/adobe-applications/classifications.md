---
keywords: Experience Platform；ホーム；人気のあるトピック；analytics；分類
description: UI でAdobe Analyticsソースコネクタを作成し、分類データをAdobe Experience Platformに取り込む方法を説明します。
solution: Experience Platform
title: UI でのAdobe Analytics分類データのソース接続の作成
topic-legacy: overview
type: Tutorial
exl-id: d606720d-f1ca-47cc-919b-643a8fc61e07
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 11%

---

# UI での分類データ用のAdobe Analyticsソース接続の作成

このチュートリアルでは、UI でAdobe Analytics Classifications データソース接続を作成して、分類データをAdobe Experience Platformに取り込む手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md):Experience Platformは、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

Analytics Classifications Data Connector を使用するには、使用する前に、データをAdobe Analyticsの新しい [!DNL Classifications] インフラストラクチャに移行する必要があります。 データの移行ステータスを確認するには、担当のAdobeカスタマーサクセスマネージャーにお問い合わせください。

## 分類の選択

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択してソースワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、受信接続を作成するために使用可能なソースが表示されます。 各ソースカードには、新しいアカウントを設定するか、既存のアカウントにデータを追加するかのオプションが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL Adobeアプリケーション]**」カテゴリで、**[!UICONTROL Adobe Analytics]** カードを選択し、「**[!UICONTROL データを追加]**」を選択して、Analytics 分類データの使用を開始します。

![](../../../../images/tutorials/create/classifications/catalog.png)

**[!UICONTROL Analytics ソースのデータ追加]** 手順が表示されます。 上部のヘッダーから **[!UICONTROL 分類]** を選択して、[!DNL Classifications] データセットのリストを表示します。このリストには、ディメンション ID、レポートスイート名、レポートスイート ID に関する情報が含まれます。

各ページには、選択できる最大 10 個の [!DNL Classifications] データセットが表示されます。 ページ下部の「**[!UICONTROL 次へ]**」を選択して、さらに多くのオプションを参照します。 右側のパネルには、選択した [!DNL Classifications] データセットの合計数と名前が表示されます。 また、このパネルでは、誤って選択した [!DNL Classifications] データセットを削除したり、1 回の操作ですべての選択を解除したりできます。

最大 30 個の異なる [!DNL Classifications] データセットを選択して、[!DNL Platform] に取り込むことができます。

[!DNL Classifications] データセットを選択したら、ページの右上にある「**[!UICONTROL 次へ]**」を選択します。

![](../../../../images/tutorials/create/classifications/add-data.png)

## 分類のレビュー

**[!UICONTROL レビュー]** 手順が表示され、選択した [!DNL Classifications] データセットを作成前に確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースプラットフォームと接続の状態を表示します。
* **[!UICONTROL データタイプ]**:選択したの数を表示しま [!DNL Classifications]す。
* **[!UICONTROL スケジュール]**:データの同期の頻度を示し [!DNL Classifications] ます。

データフローをレビューしたら、「**[!UICONTROL 完了]**」をクリックし、データフローの作成に時間を割きます。

![](../../../../images/tutorials/create/classifications/review.png)

## 分類データフローの監視

データフローを作成したら、データフローを介して取り込まれるデータを監視できます。 **[!UICONTROL カタログ]** 画面で、**[!UICONTROL データフロー]** を選択して、[!DNL Classifications] アカウントに関連付けられた確立済みフローのリストを表示します。

![](../../../../images/tutorials/create/classifications/dataflows.png)

**[!UICONTROL データフロー]** 画面が表示されます。 このページは、データフローのリストです。データフローの名前、ソース・データおよびデータフローの実行ステータスに関する情報が含まれます。 右側は、[!DNL Classifications] データフローに関するメタデータを含む **[!UICONTROL プロパティ]** パネルです。

アクセスする **[!UICONTROL ターゲットデータセット]** を選択します。

![](../../../../images/tutorials/create/classifications/list-of-dataflows.png)

**[!UICONTROL データセットアクティビティ]** ページには、選択したターゲットデータセットに関する情報が表示されます。この情報には、バッチステータス、データセット ID、スキーマに関する情報が含まれます。

>[!IMPORTANT]
>
>データセットの削除は、他のソースコネクタでは可能ですが、Analytics 分類データコネクタでは現在サポートされていません。データセットを誤って削除した場合は、Adobe カスタマーサポートにお問い合わせください。

![](../../../../images/tutorials/create/classifications/dataset.png)


## 次の手順

このチュートリアルでは、[!DNL Classifications] データを [!DNL Platform] に取り込む Analytics 分類データコネクタを作成しました。 [!DNL Analytics] および [!DNL Classifications] データの詳細については、次のドキュメントを参照してください。

* [Analytics Data コネクタの概要](../../../../connectors/adobe-applications/analytics.md)
* [UI での Analytics データ接続の作成](./analytics.md)
* [分類について](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=ja)
