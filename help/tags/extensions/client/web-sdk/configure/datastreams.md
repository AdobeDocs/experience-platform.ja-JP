---
title: データストリーム設定
description: Web SDK タグ拡張機能を使用して、にデータを送信するデータストリームを設定します。
exl-id: 2d2504c6-b3f9-4e7b-aff4-a8d8d6c4e3dd
source-git-commit: 6c05d8abde0e4d6b07fe37d6e3eacd5d3dd67ec2
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 10%

---

# データストリーム設定 {#datastreams}

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_datastreams"
>title="データストリーム"
>abstract="必須。 データの送信先の Edge Network 内にデータストリームを設定します。"

この設定セクションでは、データの送信先となる[ データストリーム ](/help/datastreams/overview.md)を決定できます。 **Edge Networkに送信されるすべてのデータには、データストリーム IDが必要です。**

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]**&#x200B;に移動し、[!UICONTROL Adobe Experience Platform Web SDK] カードの&#x200B;**[!UICONTROL Configure]**&#x200B;を選択します。
1. **[!UICONTROL Datastreams]** セクションまでスクロールします。

![ タグ UI](../assets/web-sdk-ext-datastreams.png)のWeb SDK タグ拡張機能のデータストリーム設定を示す画像

データストリームを選択する場合は、各[環境](/help/tags/ui/publishing/environments.md) （[!UICONTROL Development]、[!UICONTROL Staging]、および[!UICONTROL Production]）に対して行うことができます。 これらのフィールドは、開発環境、ステージング環境、実稼動環境の間で送信されるデータを分離する場合に役立ちます。 各環境に適切なタグローダーをインストールすれば、間違ったデータストリームにデータを送信する心配がない便利なワークフローを実現できます。

次のいずれかの方法を使用して、データストリーム IDを入力できます。

* **[!UICONTROL Choose from list]**：各環境には2つのドロップダウンメニューがあり、選択した環境のサンドボックスとデータストリームを選択できます。 各ドロップダウンメニューの値は、各[ サンドボックス ](/help/sandboxes/ui/overview.md)内の設定された[ データストリーム ](/help/datastreams/overview.md)によって異なります。

* **[!UICONTROL Enter values]**: ドロップダウンメニューを使用して目的のデータストリームを選択する代わりに、目的のデータストリーム IDを手動で直接指定できます。 各環境では、データストリーム IDを直接入力したり、[ データ要素](/help/tags/ui/managing-resources/data-elements.md)を使用してこのフィールドに入力したりできます。
