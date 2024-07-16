---
keywords: Experience Platform；ホーム；人気のトピック；Audience Manager ソースコネクタ；Audience Manager;Audience Manager コネクタ
title: UI でのAdobe Audience Manager Source接続の作成
description: このチュートリアルでは、Adobe Audience Managerのソース接続を作成し、ユーザーインターフェイスを使用してコンシューマーエクスペリエンスイベントデータを Platform に取り込む手順について説明します。
exl-id: 90c4a719-aaad-4687-afd8-7a1c0c56f744
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 29%

---

# UI での Adobe Audience Manager ソース接続の作成

このチュートリアルでは、Adobe Audience Managerのソースコネクタを作成し、ユーザーインターフェイスを使用して Consumer Experience Event データを Platform に取り込む手順について説明します。

## Adobe Audience Managerとのソース接続の作成

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL Adobeアプリケーション ] で、「**[!UICONTROL Adobe Audience Manager]**」を選択し、次に「**[!UICONTROL 設定]**」を選択します。

![カタログ](../../../../images/tutorials/create/aam/catalog.png)

### 特性およびセグメントを選択

>[!NOTE]
>
>地域データをAudience ManagerソースからExperience Platformに取り込むことはできません。 地域データを必要とする Analytics のユースケースがある場合は、[Analytics ソースコネクタ ](../adobe-applications/analytics.md) を使用してください。

[!UICONTROL  特性とセグメントを選択 ] 手順が表示され、特性、セグメントおよびデータを調査して選択するためのインタラクティブインターフェイスが利用できます。

* インターフェイスの左側のパネルには、[!UICONTROL  特性とセグメントを選択 ] オプションと、使用可能なすべてのセグメントの階層ディレクトリが含まれています。
* インターフェイスの右半分を使用すると、選択したセグメントを操作し、使用する特定のデータを選択できます。

![add-data](../../../../images/tutorials/create/aam/add-data.png)

使用可能なセグメント間を移動するには、アクセスするフォルダーを [!UICONTROL  すべてのセグメント ] パネルから選択します。 フォルダーを選択すると、フォルダーの階層をトラバースでき、フィルタリングするセグメントのリストが表示されます。

![segment-folder](../../../../images/tutorials/create/aam/segment-folder.png)

使用するセグメントを特定して選択すると、新しいパネルが右側に表示され、選択した項目のリストが表示されます。 引き続き様々なフォルダーにアクセスし、接続に異なるセグメントを選択できます。 さらにセグメントを選択すると、右側のパネルが更新されます。

![select-data](../../../../images/tutorials/create/aam/select-data.png)

または、「**[!UICONTROL すべてのセグメントを選択]**」および「**[!UICONTROL すべての特性を選択]**」ボックスを選択できます。 すべてのセグメントを選択すると、Audience Managerのセグメントが Platform に取り込まれますが、すべての特性を選択すると、Audience Managerのすべてのファーストパーティ特性が有効になります。

>[!WARNING]
>
>サイズの大きい Audience Manager セグメント母集団の取り込みは、Audience Manager ソースを使用して初めて Platform に Audience Manager セグメントを送信する際に、合計プロファイル数に直接影響します。つまり、すべてのセグメントを選択すると、ライセンス使用権限を超えるプロファイル数が発生する可能性があります。続行する前に、[ ライセンス使用許可 ](../../../../../dashboards/guides/license-usage.md) を確認してください。

完了したら、「**[!UICONTROL 次へ]**」を選択します

![all-segments](../../../../images/tutorials/create/aam/all-segments.png)

[!UICONTROL  レビュー ] 手順が表示され、選択した特性とセグメントを Platform に接続する前にレビューすることができます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースプラットフォームと接続のステータスを表示します。
* **[!UICONTROL 選択したデータ]**：選択したセグメントと有効な特性の数が表示されます。

![レビュー](../../../../images/tutorials/create/aam/review.png)

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

## 次の手順

Audience Managerデータフローがアクティブな間、受信データはリアルタイム顧客プロファイルに自動的に取り込まれます。 この受信データを利用して、Platform セグメント化サービスを使用してオーディエンスセグメントを作成できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
* [セグメント化サービスの概要](../../../../../segmentation/home.md)
