---
keywords: Experience Platform；ホーム；人気の高いトピック；analytics；分類
description: 分類データをAdobe Experience Platformに取り込むためのAdobe AnalyticsソースコネクタUIを作成する方法を説明します。
solution: Experience Platform
title: UIで分類データのAdobe Analyticsソース接続を作成する
topic-legacy: overview
type: Tutorial
exl-id: d606720d-f1ca-47cc-919b-643a8fc61e07
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 3%

---

# UIで分類データのAdobe Analyticsソース接続を作成する

このチュートリアルでは、UIで分類データをAdobe Experience Platformに取り込むためのAdobe Analytics分類データソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md):Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Analytics Classifications Data Connectorを使用するには、使用する前に、データを新しいAdobe Analyticsの[!DNL Classifications]インフラストラクチャに移行する必要があります。 データの移行ステータスを確認するには、Adobeのカスタマーサクセスマネージャーにお問い合わせください。

## 分類の選択

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択してソースワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、受信接続を作成するための利用可能なソースが表示されます。 各ソースカードには、新しいアカウントを設定するか、既存のアカウントにデータを追加するかのオプションが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Adobeアプリ]**&#x200B;カテゴリーの下で、**[!UICONTROL Adobe Analytics]**&#x200B;カードを選択し、**[!UICONTROL 追加データ]**&#x200B;を選択して、Analytics分類データの操作を開始します。

![](../../../../images/tutorials/create/classifications/catalog.png)

**[!UICONTROL Analyticsソースのデータを追加]**&#x200B;の手順が表示されます。 上部のヘッダーで「**[!UICONTROL 分類]**」を選択すると、ディメンションID、レポートスイート名、レポートスイートIDに関するリストを含む[!DNL Classifications]データセットの情報が表示されます。

各ページには、10個までの[!DNL Classifications]データセットが表示されます。 ページの下部にある「**[!UICONTROL 次へ]**」を選択して、他のオプションを参照します。 右側のパネルには、選択した[!DNL Classifications]データセットの総数と名前が表示されます。 このパネルでは、誤って選択した[!DNL Classifications]データセットを削除したり、1回の操作ですべての選択を解除したりすることもできます。

[!DNL Platform]に取り込む[!DNL Classifications]データセットは最大30個まで選択できます。

[!DNL Classifications]データセットを選択したら、ページの右上にある「**[!UICONTROL 次へ]**」を選択します。

![](../../../../images/tutorials/create/classifications/add-data.png)

## 分類の確認

**[!UICONTROL レビュー]**&#x200B;の手順が表示され、選択した[!DNL Classifications]データセットを作成前に確認できます。 詳細は次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:接続元プラットフォームと接続の状態を表示します。
* **[!UICONTROL データタイプ]**:選択した数が表示され [!DNL Classifications]ます。
* **[!UICONTROL スケジュール]**:デー [!DNL Classifications] タの同期の頻度を示します。

データフローを確認したら、**[!UICONTROL 「Finish]**」をクリックし、データフローの作成に時間を割り当てます。

![](../../../../images/tutorials/create/classifications/review.png)

## 分類のデータフローの監視

データフローを作成したら、データフローを介して取り込まれるデータを監視できます。 **[!UICONTROL カタログ]**&#x200B;画面で、**[!UICONTROL データフロー]**&#x200B;を選択し、[!DNL Classifications]アカウントに関連付けられた確立済みフローのリストを表示します。

![](../../../../images/tutorials/create/classifications/dataflows.png)

**[!UICONTROL Dataflows]**&#x200B;画面が表示されます。 このページは、名前、ソース・データおよびデータ・フロー実行ステータスに関するリストを含むデータ・フローの情報です。 右側は、[!DNL Classifications]データフローに関するメタデータを含む&#x200B;**[!UICONTROL プロパティ]**&#x200B;パネルです。

アクセスする&#x200B;**[!UICONTROL ターゲットデータセット]**&#x200B;を選択します。

![](../../../../images/tutorials/create/classifications/list-of-dataflows.png)

**[!UICONTROL データセットアクティビティ]**&#x200B;ページには、選択したターゲットデータセットに関する情報が表示されます。この情報には、バッチステータス、データセットID、スキーマに関する詳細な情報が含まれます。

>[!IMPORTANT]
>
>他のソースコネクターではデータセットの削除は可能ですが、現在、Analytics分類データコネクターではサポートされていません。 誤ってデータセットを削除した場合は、Adobeカスタマーケアにお問い合わせください。

![](../../../../images/tutorials/create/classifications/dataset.png)


## 次の手順

このチュートリアルに従って、[!DNL Classifications]データを[!DNL Platform]に取り込むAnalytics分類データコネクタを作成しました。 [!DNL Analytics]と[!DNL Classifications]のデータについて詳しくは、次のドキュメントを参照してください。

* [Analytics Data Connectorの概要](../../../../connectors/adobe-applications/analytics.md)
* [UIでのAnalyticsデータ接続の作成](./analytics.md)
* [分類について](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html)
