---
keywords: 宛先の削除、宛先の削除方法、宛先の削除
title: 宛先の削除
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform UI で既存の宛先を削除する手順を示します
exl-id: 7b672859-e61a-4b3c-9db9-62048258f0aa
source-git-commit: a97b235e2d8834f6be002923be9cdbca5f08495b
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 1%

---

# 宛先の削除 {#delete-destinations}

## 概要 {#overview}

Adobe Experience Platformユーザーインターフェイスで、宛先への既存の接続を削除できます。

宛先を削除すると、その宛先への既存のデータフローが削除されます。 削除する宛先に対してアクティブ化されたすべてのセグメントは、データフローが削除される前にマッピングされなくなります。

[!DNL Platform] [!DNL UI] から宛先を削除する方法は 2 つあります。 次のことができます。

* [「[!UICONTROL  参照 ]」タブからの宛先の削除](#delete-browse-tab)
* [宛先の詳細ページからの宛先の削除](#delete-destination-details-page)

>[!IMPORTANT]
>
>宛先 *への既存の* 接続は削除できますが、現在、Platform では既存の *[宛先アカウント](/help/destinations/ui/destinations-workspace.md#accounts)* を削除することはできません。

## 「参照」タブからの宛先の削除{#delete-browse-tab}

「[!UICONTROL  参照 ]」タブから宛先を削除するには、次の手順に従います。

1. [Experience PlatformUI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから **[!UICONTROL 宛先]** を選択します。 既存の宛先を表示するには、上部のヘッダーから「**[!UICONTROL 参照]**」を選択します。

   ![宛先の参照](../assets/ui/delete-destinations/browse-destinations.png)

2. 左上のフィルターアイコン ![ フィルターアイコン ](../assets/ui/delete-destinations/filter.png) を選択して、並べ替えパネルを起動します。 並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられたデータフローのフィルタ選択を表示できます。

   ![宛先のフィルター](../assets/ui/delete-destinations/filter-destinations.png)

3. 「名前」列で「![ さらにボタン ](../assets/ui/delete-destinations/more-icon.png)」を選択し、「![ 削除ボタン ](../assets/ui/delete-destinations/delete-icon.png) **[!UICONTROL 削除]**」を選択して、既存の宛先接続を削除します。
   ![宛先の削除](../assets/ui/delete-destinations/delete-destinations.png)

4. 「**[!UICONTROL 削除]**」を選択して、宛先接続の削除を確認します。

   ![宛先の削除の確認](../assets/ui/delete-destinations/delete-destinations-confirm.png)


## 宛先の詳細ページからの宛先の削除{#delete-destination-details-page}

宛先の詳細ページから宛先を削除するには、以下の手順に従います。

1. [Experience PlatformUI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから **[!UICONTROL 宛先]** を選択します。 既存の宛先を表示するには、上部のヘッダーから「**[!UICONTROL 参照]**」を選択します。

   ![宛先の参照](../assets/ui/delete-destinations/browse-destinations.png)

2. 左上のフィルターアイコン ![ フィルターアイコン ](../assets/ui/delete-destinations/filter.png) を選択して、並べ替えパネルを起動します。 並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられたデータフローのフィルタ選択を表示できます。

   ![宛先のフィルター](../assets/ui/delete-destinations/filter-destinations.png)

3. 削除する宛先の名前を選択します。

   ![宛先の選択](../assets/ui/delete-destinations/delete-destination-select.png)

   * 宛先に既存のデータフローがある場合は、「[!UICONTROL Dataflow runs]」タブに移動します。

      ![「Dataflow runs」タブ](../assets/ui/delete-destinations/destination-details-dataflows.png)

   * 宛先に既存のデータフローがない場合は、空のページが表示され、オーディエンスのアクティブ化を開始できます。

      ![宛先の詳細](../assets/ui/delete-destinations/destination-details-empty.png)


4. 右側のレールで **[!UICONTROL 削除]** を選択します。

   ![宛先の削除](../assets/ui/delete-destinations/delete-destinations-button.png)

5. 確認ダイアログで「**[!UICONTROL 削除]**」を選択して、宛先を削除します。

   ![宛先の削除の確認](..//assets/ui/delete-destinations/delete-destinations-delete.png)

   >[!NOTE]
   >
   >サーバーの負荷によっては、[!DNL Platform] が宛先を削除するまでに数分かかる場合があります。
