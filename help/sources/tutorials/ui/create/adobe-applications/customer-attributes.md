---
keywords: Experience Platform；ホーム；人気の高いトピック；顧客属性
solution: Experience Platform
title: UIでの顧客属性ソース接続の作成
topic: overview
type: Tutorial
description: 顧客属性プロファイルデータをAdobe Experience Platformに収集するためのUIでソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: 08a3026e969a8739a8b57226c35a6d1d3150006e
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 5%

---


# UIでの顧客属性ソース接続の作成

このチュートリアルでは、顧客属性プロファイルデータをAdobe Experience Platformに収集するためのソース接続をUIで作成する手順を説明します。 顧客属性について詳しくは、[顧客属性の概要](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/attributes.html)を参照してください。

>[!IMPORTANT]
>
>データフローの無効化、有効化、削除の機能は、現在、顧客属性ソースではサポートされていません。

## ソース接続の作成

プラットフォームUIで、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、接続を作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL Adobeアプリケーション]カテゴリーで、「**[!UICONTROL 顧客属性]**」を選択し、追加「**[!UICONTROL データ]**」を選択します。

>[!NOTE]
>
>顧客属性プロファイルデータのソース接続を既に確立している場合、ソースとの接続オプションは無効になります。

![](../../../../images/tutorials/create/customer-attributes/catalog.png)

[!UICONTROL 追加data]画面には、顧客属性に使用できるすべてのデータソースがリストされます。 新しい接続を作成するには、リストからデータソースを選択し、「**[!UICONTROL 次へ]**」を選択します。

>[!NOTE]
>
>顧客属性ソース接続ごとに選択できるデータセットは1つだけです。

![](../../../../images/tutorials/create/customer-attributes/add-data.png)

[!UICONTROL Dataflow detail]ステップが表示され、新しいデータフローに名前を付け、簡単な説明を入力できます。

このプロセス中に、[!UICONTROL 部分的な取り込み]と[!UICONTROL エラー診断]を有効にすることもできます。 [!UICONTROL 部分] 的な取り込みでは、エラーを含むデータを取り込む機能を提供します。設定可能なしきい値まで取り込むことができます。一方、 [!UICONTROL エラー] 診断では、個別にバッチ処理される誤ったデータの詳細が提供されます。詳しくは、[部分的なバッチインジェストの概要](../../../../../ingestion/batch-ingestion/partial.md)を参照してください。

![](../../../../images/tutorials/create/customer-attributes/dataflow-detail.png)

「[!UICONTROL レビュー]」ステップが表示され、新しいデータフローを作成前に確認できます。 詳細は次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースの種類、選択したソースファイルの関連パス、およびそのソースファイル内の列数が表示されます。
* **[!UICONTROL データセットとマップのフィールドの割り当て]**:ソースデータが取り込まれるデータセット(データセットに従うスキーマなど)を示します。

![](../../../../images/tutorials/create/customer-attributes/review.png)

## 次の手順

接続が作成されると、ターゲットスキーマとデータセットが自動的に作成され、受信データが格納されます。初回取り込みが完了すると、顧客属性プロファイルデータは、[!DNL Real-time Customer Profile]や[!DNL Segmentation Service]などの下流のプラットフォームサービスで使用できます。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] 概要](../../../../../segmentation/home.md)
