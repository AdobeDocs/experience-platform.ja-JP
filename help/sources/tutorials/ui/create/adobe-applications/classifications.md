---
description: UI を使用してAdobe Analytics ソースコネクタを作成し、分類データをAdobe Experience Platformに取り込む方法を説明します。
title: UI で、分類データのAdobe Analytics Source接続を作成します
exl-id: d606720d-f1ca-47cc-919b-643a8fc61e07
source-git-commit: dfc8a1d51e6dd25210a0b6f24dad4d0f00052414
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 16%

---

# UI で、分類データのAdobe Analytics ソース接続を作成します

>[!TIP]
>
>デフォルトでは、Adobe Analyticsの分類データは毎週更新されます。 分類データのデータ取り込みは、データフローの初回設定から 7 日後に処理されます。 最初の読み込みはデータ全体を取り込み、その後の週次取り込みは増分データを実行します。

ユーザーインターフェイスを使用してAdobe Analytics分類データをAdobe Experience Platformに取り込む手順については、このチュートリアルをお読みください。

## 基本を学ぶ

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイム顧客プロファイルを提供します。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

Analytics Classifications ソースコネクタでは、使用前にデータをAdobe Analyticsの新しい分類インフラストラクチャに移行しておく必要があります。 データの移行ステータスを確認するには、Adobe アカウントチームにお問い合わせください。

## 分類を選択

Experience Platformの UI で、左側のナビゲーションから「**[!UICONTROL Sources]**」を選択して、[!UICONTROL Sources] ワークスペースにアクセスします。 画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*Adobe アプリケーション* カテゴリで、**[!UICONTROL Adobe Analytics]** を選択し、次に **[!UICONTROL Set up]** を選択します。

>[!TIP]
>
>認証済みアカウントがない場合、ソースカタログ内のソースに「**[!UICONTROL Set up]**」オプションが表示されます。 アカウントが認証されると、オプションが **[!UICONTROL Add data]** に変わります。

![Experience Platform UI のソースカタログで、Adobe Analytics ソースが選択された状態。](../../../../images/tutorials/create/classifications/catalog.png)

次に「[!UICONTROL Classifications]」を選択し、Experience Platformに取り込む分類データセットを選択します。 または、検索を使用して、特定の分類をフィルタリングして選択することもできます。

最大 30 個の異なる分類データセットを選択して、Experience Platformに取り込むことができます。 選択したデータセットは、右側のパネルに表示されます。 完了したら、「[!UICONTROL Next]」を選択して続行します。

![ 複数の分類データセットが選択された分類ページ ](../../../../images/tutorials/create/classifications/select.png)

## 分類をレビュー

**[!UICONTROL Review]** の手順が表示され、選択した分類データセットを作成前に確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL Connection]**：ソースプラットフォームと接続のステータスが表示されます。
* **[!UICONTROL Data type]**：選択した分類の数が表示されます。
* **[!UICONTROL Scheduling]**：分類データの同期の頻度を示します。 **メモ**：分類データは毎週更新されます。

データフローをレビューしたら、「**[!UICONTROL Finish]**」をクリックし、データフローが作成されるまでしばらく待ちます。

![Adobe Analytics分類データのレビューページ ](../../../../images/tutorials/create/classifications/review.png)

## 次の手順

このチュートリアルでは、分類データをExperience Platformに取り込む Analytics classifications sata コネクタを作成しました。 [!DNL Analytics] と分類データについて詳しくは、次のドキュメントを参照してください。

* [Adobe Analytics ソースコネクタの概要](../../../../connectors/adobe-applications/analytics.md)
* [UI でのレポートスイートデータの Analytics ソース接続の作成](./analytics.md)
* [ 分類について ](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html)
