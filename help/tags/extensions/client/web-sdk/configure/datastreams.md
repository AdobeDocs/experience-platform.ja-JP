---
title: データストリーム設定
description: Web SDK タグ拡張機能を使用して、にデータを送信するデータストリームを設定します。
source-git-commit: 46e5d007b27eaa67c9ee49e35a711424de383d68
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 1%

---

# データストリーム設定

この設定セクションでは、データの送信先の [&#x200B; データストリーム &#x200B;](/help/datastreams/overview.md) を決定できます。 **Edge Networkに送信するすべてのデータにデータストリーム ID が必要です。**

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Datastreams]** セクションまで下にスクロールします。

![&#x200B; タグ UI の Web SDK タグ拡張機能のデータストリーム設定を示す画像 &#x200B;](../assets/web-sdk-ext-datastreams.png)

データストリームを選択する際は、[&#x200B; 環境 &#x200B;](/help/tags/ui/publishing/environments.md) （[!UICONTROL Development]、[!UICONTROL Staging] および [!UICONTROL Production]）ごとに選択できます。 これらのフィールドは、開発環境、ステージング環境および実稼動環境の間で送信されるデータを分離する場合に役立ちます。 これにより、各環境に正しいタグローダーをインストールする限り、間違ったデータストリームへのデータの送信を心配する必要がない、便利なワークフローが可能になります。

次のいずれかの方法を使用して、データストリーム ID を入力できます。

* **[!UICONTROL Choose from list]**：各環境にはドロップダウンメニューが 2 つ含まれ、選択した環境のサンドボックスとデータストリームを選択できます。 各ドロップダウンメニューの値は、それぞれの [&#x200B; サンドボックス &#x200B;](/help/datastreams/overview.md) 内で設定した [&#x200B; データストリーム &#x200B;](/help/sandboxes/ui/overview.md) によって異なります。

* **[!UICONTROL Enter values]**：ドロップダウンメニューを使用して目的のデータストリームを選択する代わりに、目的のデータストリーム ID を手動で直接指定できます。 各環境では、データストリーム ID を直接入力したり、[data 要素 &#x200B;](/help/tags/ui/managing-resources/data-elements.md) を使用してこのフィールドに値を入力したりできます。
