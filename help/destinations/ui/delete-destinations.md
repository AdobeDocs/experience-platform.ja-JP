---
keywords: 宛先の削除、宛先の削除方法、宛先の削除
title: 宛先の削除
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform UI で既存の宛先を削除する手順を示します
exl-id: 7b672859-e61a-4b3c-9db9-62048258f0aa
source-git-commit: 1ef6430b6661a2b8b5aef196b75cfaf3f6220aab
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 33%

---

# 宛先の削除 {#delete-destinations}

## 概要 {#overview}

Adobe Experience Platformユーザーインターフェイスで、宛先への既存の接続を削除できます。

宛先を削除すると、その宛先への既存のデータフローが削除されます。 削除する宛先に対してアクティブ化されているセグメントは、データフローが削除される前にマッピング解除されます。

から宛先を削除する方法は 2 つあります [!DNL Platform] [!DNL UI]. 次のことができます。

* [からの宛先の削除 [!UICONTROL 参照] タブ](#delete-browse-tab)
* [宛先の詳細ページからの宛先の削除](#delete-destination-details-page)

## 「参照」タブからの宛先の削除{#delete-browse-tab}

次の手順に従って、 [!UICONTROL 参照] タブをクリックします。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。既存の宛先を表示するには、「 」を選択します。 **[!UICONTROL 参照]** を上部のヘッダーから削除します。

   ![宛先の参照](../assets/ui/delete-destinations/browse-destinations.png)

2. 左上のフィルターアイコン ![フィルターアイコン](../assets/ui/delete-destinations/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![宛先のフィルタリング](../assets/ui/delete-destinations/filter-destinations.png)

3. を選択します。 ![「詳細」ボタン](../assets/ui/delete-destinations/more-icon.png) 「名前」列の「 」ボタンをクリックし、 ![削除ボタン](../assets/ui/delete-destinations/delete-icon.png) **[!UICONTROL 削除]** 既存の宛先接続を削除する場合。
   ![宛先の削除](../assets/ui/delete-destinations/delete-destinations.png)

4. 選択 **[!UICONTROL 削除]** をクリックして、宛先接続の削除を確認します。

   ![宛先の削除を確認](../assets/ui/delete-destinations/delete-destinations-confirm.png)

## 宛先の詳細ページからの宛先の削除{#delete-destination-details-page}

宛先の詳細ページから宛先を削除するには、以下の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。既存の宛先を表示するには、「 」を選択します。 **[!UICONTROL 参照]** を上部のヘッダーから削除します。

   ![宛先の参照](../assets/ui/delete-destinations/browse-destinations.png)

2. 左上のフィルターアイコン ![フィルターアイコン](../assets/ui/delete-destinations/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![宛先のフィルタリング](../assets/ui/delete-destinations/filter-destinations.png)

3. 削除する宛先の名前を選択します。

   ![宛先を選択](../assets/ui/delete-destinations/delete-destination-select.png)

   * 宛先に既存のデータフローがある場合は、「 [!UICONTROL データフローの実行] タブをクリックします。

      ![「データフロー実行」タブ](../assets/ui/delete-destinations/destination-details-dataflows.png)

   * 宛先に既存のデータフローがない場合は、空のページが表示され、オーディエンスのアクティブ化を開始できます。

      ![宛先の詳細](../assets/ui/delete-destinations/destination-details-empty.png)

4. 選択 **[!UICONTROL 削除]** をクリックします。

   ![宛先の削除](../assets/ui/delete-destinations/delete-destinations-button.png)

5. 選択 **[!UICONTROL 削除]** をクリックして、宛先を削除します。

   ![宛先の削除の確認](..//assets/ui/delete-destinations/delete-destinations-delete.png)

   >[!NOTE]
   >
   >サーバーの負荷によっては、 [!DNL Platform] をクリックして、宛先を削除します。
