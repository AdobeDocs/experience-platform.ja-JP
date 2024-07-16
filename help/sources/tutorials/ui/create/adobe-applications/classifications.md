---
keywords: Experience Platform；ホーム；人気のトピック；Analytics；分類
description: UI を使用してAdobe Analytics ソースコネクタを作成し、分類データをAdobe Experience Platformに取り込む方法を説明します。
solution: Experience Platform
title: UI で、分類データのAdobe Analytics Source接続を作成します
type: Tutorial
exl-id: d606720d-f1ca-47cc-919b-643a8fc61e07
source-git-commit: fcebef97ba9cc667f80afd55980c5460912a56fb
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 15%

---

# UI で、分類データのAdobe Analytics ソース接続を作成します

このチュートリアルでは、UI でAdobe Analytics Classifications Data ソース接続を作成し、分類データをAdobe Experience Platformに取り込む手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイム顧客プロファイルを提供します。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md): Experience Platformには、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

Analytics Classifications Data Connector では、使用前にデータをAdobe Analyticsの新しい [!DNL Classifications] インフラストラクチャに移行しておく必要があります。 データの移行状況を確認するには、Adobeアカウントチームにお問い合わせください。

## 分類を選択

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して、ソース ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、インバウンド接続を作成するための使用可能なソースが表示されます。 各ソースカードには、新しいアカウントを設定するか、既存のアカウントにデータを追加するかのオプションが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して見つけることもできます。

**[!UICONTROL Adobeアプリケーション]** カテゴリで、**[!UICONTROL Adobe Analytics]** カードを選択し、「**[!UICONTROL データを追加]**」を選択して Analytics Classifications Data の使用を開始します。

![](../../../../images/tutorials/create/classifications/catalog.png)

**[!UICONTROL Analytics ソースデータの追加]**&#x200B;の手順が表示されます。上部のヘッダーから **[!UICONTROL 分類]** を選択して、ディメンション ID、レポートスイート名、レポートスイート ID に関する情報を含む、[!DNL Classifications] のデータセットのリストを表示します。

各ページには、選択可能な最大 10 個の異なる [!DNL Classifications] データセットが表示されます。 ページの下部にある「**[!UICONTROL 次へ]**」を選択して、その他のオプションを参照します。 右側のパネルには、選択したデータセット [!DNL Classifications] 合計数と名前が表示されます。 また、このパネルでは、誤って選択した [!DNL Classifications] のデータセットを削除したり、1 つのアクションですべての選択をクリアしたりできます。

最大 30 個の異なる [!DNL Classifications] データセットを選択して、[!DNL Platform] に取り込むことができます。

[!DNL Classifications] データセットを選択したら、ページの右上にある **[!UICONTROL 次へ]** を選択します。

![](../../../../images/tutorials/create/classifications/add-data.png)

## 分類をレビュー

**[!UICONTROL レビュー]** 手順が表示され、選択した [!DNL Classifications] データセットを作成前にレビューできます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースプラットフォームと接続のステータスを表示します。
* **[!UICONTROL データタイプ]**：選択した [!DNL Classifications] の数が表示されます。
* **[!UICONTROL スケジュール設定]**：データの同期の頻度 [!DNL Classifications] 表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」をクリックし、データフローが作成されるまでしばらく待ちます。

![](../../../../images/tutorials/create/classifications/review.png)

## 分類データフローの監視

データフローを作成したら、それを通じて取り込まれるデータを監視できます。 **[!UICONTROL カタログ]** 画面から「**[!UICONTROL データフロー]**」を選択すると、[!DNL Classifications] アカウントに関連する確立済みフローのリストが表示されます。

![](../../../../images/tutorials/create/classifications/dataflows.png)

**[!UICONTROL データフロー]**&#x200B;画面が表示されます。 このページには、名前、ソースデータ、データフローの実行ステータスに関する情報を含む、データフローのリストが表示されます。 右側には、[!DNL Classifications] しいデータフローに関するメタデータを含む **[!UICONTROL プロパティ]** パネルがあります。

アクセスする **[!UICONTROL ターゲットデータセット]** を選択します。

![](../../../../images/tutorials/create/classifications/list-of-dataflows.png)

**[!UICONTROL データセットアクティビティ]** ページには、バッチステータス、データセット ID、スキーマに関する詳細など、選択したターゲットデータセットに関する情報が表示されます。

![](../../../../images/tutorials/create/classifications/dataset.png)

## 次の手順

このチュートリアルでは、[!DNL Classifications] しいデータを [!DNL Platform] に取り込む Analytics 分類データコネクタを作成しました。 [!DNL Analytics] および [!DNL Classifications] データについて詳しくは、次のドキュメントを参照してください。

* [Analytics Data Connector の概要](../../../../connectors/adobe-applications/analytics.md)
* [UI での Analytics データ接続の作成](./analytics.md)
* [ 分類について ](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html)
