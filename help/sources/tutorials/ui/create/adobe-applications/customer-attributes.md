---
keywords: Experience Platform；ホーム；人気のあるトピック；顧客属性
solution: Experience Platform
title: UI での顧客属性ソース接続の作成
topic-legacy: overview
type: Tutorial
description: 顧客属性プロファイルデータをAdobe Experience Platformに収集するための UI でソース接続を作成する方法を説明します。
exl-id: 66bdab8f-c00e-4ebe-8b8e-f9e12cf86bbe
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 6%

---

# UI での顧客属性ソース接続の作成

このチュートリアルでは、顧客属性プロファイルデータをAdobe Experience Platformに収集するための UI でソース接続を作成する手順を説明します。 顧客属性の詳細については、[ 顧客属性の概要 ](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/attributes.html?lang=ja) を参照してください。

>[!IMPORTANT]
>
>データフローの無効化、有効化、削除の各機能は、現在、顧客属性ソースではサポートされていません。

## ソース接続の作成

Platform UI で、左のナビゲーションから「 **[!UICONTROL ソース]** 」を選択して、「 [!UICONTROL  ソース ] 」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、接続を作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

「[!UICONTROL Adobeのアプリケーション ]」カテゴリで「**[!UICONTROL 顧客属性]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

>[!NOTE]
>
>顧客属性プロファイルデータのソース接続を既に確立している場合、ソースに接続するオプションは無効になります。

![](../../../../images/tutorials/create/customer-attributes/catalog.png)

[!UICONTROL  データを追加 ] 画面には、顧客属性で使用可能なすべてのデータソースが表示されます。 新しい接続を作成するには、リストからデータソースを選択し、「**[!UICONTROL 次へ]**」を選択します。

>[!NOTE]
>
>顧客属性ソース接続ごとに選択できるデータセットは 1 つだけです。

![](../../../../images/tutorials/create/customer-attributes/add-data.png)

[!UICONTROL  データフローの詳細 ] 手順が表示され、新しいデータフローに名前を付け、簡単な説明を入力できます。

このプロセスの間に、[!UICONTROL  部分取得 ] と [!UICONTROL  エラー診断 ] を有効にすることもできます。 [!UICONTROL 部分的] な取り込みでは、設定可能なしきい値までエラーを含むデータを取り込み、エラー診断では、個別にバッチ処理される誤ったデータの詳細を提供しま  す。詳しくは、「[ バッチ取得の部分の概要 ](../../../../../ingestion/batch-ingestion/partial.md)」を参照してください。

![](../../../../images/tutorials/create/customer-attributes/dataflow-detail.png)

「[!UICONTROL  レビュー ]」手順が表示され、新しいデータフローを作成前に確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースの種類、選択したソースファイルの関連パス、およびそのソースファイル内の列数を表示します。
* **[!UICONTROL データセットとマップのフィールドの割り当て]**:データセットが準拠するスキーマなど、ソースデータの取り込み先のデータセットを示します。

![](../../../../images/tutorials/create/customer-attributes/review.png)

## 次の手順

接続が作成されると、ターゲットスキーマとデータセットが自動的に作成され、受信データが格納されます。初回の取り込みが完了すると、顧客属性プロファイルデータは、[!DNL Real-time Customer Profile] や [!DNL Segmentation Service] など、ダウンストリームの Platform サービスで使用できます。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] の概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] の概要](../../../../../segmentation/home.md)
