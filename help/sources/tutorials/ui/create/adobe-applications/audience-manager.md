---
keywords: Experience Platform；ホーム；人気の高いトピック；オーディエンスマネージャのソースコネクタ；Audience Manager;オーディエンスマネージャのコネクタ
solution: Experience Platform
title: UIでのAdobe Audience Managerソース接続の作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、ユーザーインターフェイスを使用してコンシューマーエクスペリエンスイベントデータをプラットフォームに取り込むための、Adobe Audience Manager用のソースコネクタを作成する手順を説明します。
exl-id: 90c4a719-aaad-4687-afd8-7a1c0c56f744
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 2%

---

# UIでのAdobe Audience Managerソース接続の作成

このチュートリアルでは、ユーザーインターフェイスを使用してコンシューマーエクスペリエンスイベントデータをプラットフォームに取り込むための、Adobe Audience Manager用のソースコネクタを作成する手順を説明します。

## Adobe Audience Managerとのソース接続の作成

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

[!UICONTROL Adobeアプリケーション]カテゴリの下で、**[!UICONTROL Adobe Audience Manager]**&#x200B;を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![カタログ](../../../../images/tutorials/create/aam/catalog.png)

「[!UICONTROL 特性とセグメントを選択]」の手順が表示され、特性、セグメントおよびデータを調査および選択するためのインタラクティブなインターフェイスが提供されます。

* インターフェイスの左のパネルには、「特性を選択」および「セグメントを選択」]オプションと、自分が使用できるすべてのセグメントの階層ディレクトリが含まれます。[!UICONTROL 
* インターフェイスの右半分を使用すると、選択したセグメントを操作し、使用する特定のデータを選択できます。

![add-data](../../../../images/tutorials/create/aam/add-data.png)

使用可能なセグメント間を移動するには、[!UICONTROL すべてのセグメント]パネルからアクセスするフォルダーを選択します。 フォルダを選択すると、フォルダの階層を移動して、フィルタリングするセグメントのリストを得ることができます。

![segment-folder](../../../../images/tutorials/create/aam/segment-folder.png)

使用するセグメントを識別して選択すると、右側に新しいパネルが表示され、選択した項目のリストが表示されます。 引き続き様々なフォルダーにアクセスし、接続に応じて様々なセグメントを選択できます。 セグメントを選択すると、右側のパネルが更新されます。

![select-data](../../../../images/tutorials/create/aam/select-data.png)

または、「**[!UICONTROL すべてのセグメントを選択]**」ボックスと「**[!UICONTROL すべての特性を選択]**」ボックスを選択します。 すべてのセグメントを選択するとAudience Managerセグメントがプラットフォームに表示され、すべての特性を選択すると、Audience Managerからすべてのファーストパーティの特性が有効になります。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![全セグメント](../../../../images/tutorials/create/aam/all-segments.png)

[!UICONTROL レビュー]の手順が表示され、選択した特性とセグメントがプラットフォームに接続される前に確認できます。 詳細は次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:接続元プラットフォームと接続の状態を表示します。
* **[!UICONTROL 選択したデータ]**:選択したセグメントと有効な特性の数を表示します。

![レビュー](../../../../images/tutorials/create/aam/review.png)

データフローをレビューしたら、**[!UICONTROL 「完了」]**&#x200B;を選択し、データフローの作成に時間をかけます。

## 次の手順

Audience Managerデータフローがアクティブな間、受信データは自動的にリアルタイム顧客プロファイルに取り込まれます。 この受信データを利用して、Platform Segmentation Serviceを使用してオーディエンスセグメントを作成できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
* [セグメント化サービスの概要](../../../../../segmentation/home.md)
