---
keywords: Experience Platform；ホーム；人気のあるトピック；Audience Manager ソースコネクタ；Audience Manager;Audience Manager コネクタ
solution: Experience Platform
title: UI でのAdobe Audience Managerソース接続の作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、ユーザーインターフェイスを使用してAdobe Audience Managerのソースコネクタを作成し、コンシューマーエクスペリエンスイベントデータを Platform に取り込む手順を説明します。
exl-id: 90c4a719-aaad-4687-afd8-7a1c0c56f744
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 2%

---

# UI でのAdobe Audience Managerソース接続の作成

このチュートリアルでは、ユーザーインターフェイスを使用してAdobe Audience Managerのソースコネクタを作成し、コンシューマーエクスペリエンスイベントデータを Platform に取り込む手順を説明します。

## Adobe Audience Managerとのソース接続の作成

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL  ソース ]」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成するための様々なソースが表示されます。

[!UICONTROL Adobeアプリケーション ] カテゴリで、**[!UICONTROL Adobe Audience Manager]** を選択し、**[!UICONTROL 設定]** を選択します。

![カタログ](../../../../images/tutorials/create/aam/catalog.png)

[!UICONTROL  特性とセグメントの選択 ] 手順が表示され、特性、セグメントおよびデータを調査および選択するためのインタラクティブなインターフェイスが提供されます。

* インターフェイスの左のパネルには、「Select traits and segments]」オプションと、使用可能なすべてのセグメントの階層ディレクトリが含まれています。[!UICONTROL 
* インターフェイスの右半分では、選択したセグメントを操作し、使用する特定のデータを選択できます。

![add-data](../../../../images/tutorials/create/aam/add-data.png)

使用可能なセグメント間を移動するには、[!UICONTROL  すべてのセグメント ] パネルからアクセスするフォルダーを選択します。 フォルダーを選択すると、フォルダーの階層をたどって、フィルターするセグメントのリストを表示できます。

![segment-folder](../../../../images/tutorials/create/aam/segment-folder.png)

使用するセグメントを特定して選択すると、右側に新しいパネルが表示され、選択した項目のリストが表示されます。 引き続き様々なフォルダーにアクセスし、接続に対して様々なセグメントを選択できます。 さらにセグメントを選択すると、右側のパネルが更新されます。

![select-data](../../../../images/tutorials/create/aam/select-data.png)

または、「 **[!UICONTROL Select all segments]** 」ボックスと「 **[!UICONTROL Select all traits]** 」ボックスを選択します。 すべてのセグメントを選択するとAudience Managerセグメントが Platform に取り込まれ、すべての特性を選択すると、すべてのファーストパーティ特性がAudience Managerから有効になります。

完了したら、「**[!UICONTROL 次へ]**」を選択します。

![all-segments](../../../../images/tutorials/create/aam/all-segments.png)

[!UICONTROL  レビュー ] 手順が表示され、選択した特性とセグメントを、Platform に接続する前に確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースプラットフォームと接続の状態を表示します。
* **[!UICONTROL 選択したデータ]**:選択したセグメントと有効な特性の数を表示します。

![レビュー](../../../../images/tutorials/create/aam/review.png)

データフローをレビューしたら、「**[!UICONTROL Finish]**」を選択し、データフローの作成に時間を割きます。

## 次の手順

Audience Managerのデータフローがアクティブな間、受信データはリアルタイム顧客プロファイルに自動的に取り込まれます。 これで、この受信データを利用し、Platform セグメント化サービスを使用してオーディエンスセグメントを作成できます。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
* [セグメント化サービスの概要](../../../../../segmentation/home.md)
