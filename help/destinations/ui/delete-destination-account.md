---
keywords: 宛先アカウント、宛先アカウント、アカウントの削除方法の削除
title: 宛先アカウントの削除
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform UI で宛先アカウントを削除する手順を示します
exl-id: 9b39ba4b-19a4-48a8-a6f1-f860777cdb9e
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 18%

---

# 宛先アカウントの削除

## 概要 {#overview}

「**[!UICONTROL アカウント]**」タブには、様々な宛先との接続を確立した場合の詳細が表示されます。 各宛先アカウントについて取得できるすべての情報については、[&#x200B; アカウントの概要 &#x200B;](../ui/destinations-workspace.md#accounts) を参照してください。

このチュートリアルでは、不要になった宛先アカウントをExperience PlatformUI を使用して削除する手順を説明します。

![「アカウント」タブ](../assets/ui/update-accounts/destination-accounts.png)

## アカウントの削除 {#delete}

>[!TIP]
>
>宛先アカウントを削除する前に、まず、宛先アカウントに関連付けられた既存のデータフローを削除する必要があります。 既存の宛先データフローを削除するには、[UI での宛先データフローの削除 &#x200B;](./delete-destinations.md) のチュートリアルを参照してください。

既存の宛先アカウントを削除するには、次の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。上部のヘッダーから **[!UICONTROL アカウント]** を選択して、既存のアカウントを表示します。

   ![「アカウント」タブ](../assets/ui/delete-accounts/accounts-tab.png)

2. 左上のフィルターアイコン ![フィルターアイコン](/help/images/icons/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられたフィルターされたアカウントの選択を表示できます。

   ![&#x200B; 宛先のフィルタリング &#x200B;](../assets/ui/delete-accounts/filter-accounts.png)

3. 削除するアカウント名の横にある省略記号（`...`）を選択します。 ポップアップパネルが表示され、アカウントの **[!UICONTROL オーディエンスをアクティブ化]**、**[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]** するオプションが提供されます。 ![&#x200B; 削除ボタン &#x200B;](/help/images/icons/delete.png)**[!UICONTROL 削除]** ボタンを選択して、目的のアカウントを削除します。

   ![&#x200B; 宛先アカウントを削除 &#x200B;](../assets/ui/delete-accounts/delete-accounts.png)

4. 最終確認ダイアログボックスが表示されるので、「**[!UICONTROL 削除]**」を選択してプロセスを完了します。

![&#x200B; アカウントの削除を確認 &#x200B;](../assets/ui/delete-accounts/confirm-account-deletion.png)

## 次の手順

このチュートリアルでは、宛先ワークスペースを使用して既存のアカウントを正常に削除しました。

[!DNL Flow Service] API を使用してプログラムでこれらの操作を実行する手順については、[Flow Service API を使用した接続の削除 &#x200B;](../api/delete-destination-account.md) に関するチュートリアルを参照してください。
