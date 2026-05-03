---
title: 同意設定
description: タグ拡張機能のデフォルトの同意とプライバシー設定を設定します。
exl-id: 93913a8b-0351-409d-b26a-8dc2ac0296c5
source-git-commit: 6c05d8abde0e4d6b07fe37d6e3eacd5d3dd67ec2
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 13%

---

# 同意設定 {#consent}

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_consent"
>title="同意"
>abstract="他の明示的な同意環境設定が指定されていない場合に想定されるデフォルトの同意レベルを選択します。"

「**[!UICONTROL Consent]**」セクションでは、他の明示的な同意設定が指定されていない場合に想定されるデフォルトの同意レベルを選択できます。 デフォルトの同意レベルはユーザープロファイルに保存されません。

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]**&#x200B;に移動し、[!UICONTROL Adobe Experience Platform Web SDK] カードの&#x200B;**[!UICONTROL Configure]**&#x200B;を選択します。
1. **[!UICONTROL Consent]** セクションまでスクロールします。

![&#x200B; タグ UIのWeb SDK タグ拡張機能のプライバシー設定を示す画像](../assets/web-sdk-ext-privacy.png)

このセクションには、デフォルトの同意レベルを決定するラジオボタンのセットが1つ含まれています。

* **[!UICONTROL In]**: ユーザーが同意設定を提供する前に発生したイベントを収集します。
* **[!UICONTROL Out]**: ユーザーが同意設定を提供する前に発生したイベントをドロップします。
* **[!UICONTROL Pending]**: ユーザーが同意設定を提供する前に発生したイベントをキューに入れます。 同意が付与されると、キューに入れられたイベントはAdobeに送信されます。 同意が拒否されると、キューに入れたイベントは破棄されます。
* **[!UICONTROL Provide a data element]**：上記の設定設定のいずれかを決定するデータ要素を選択します。 有効な値には、文字列`"in"`、`"out"`または`"pending"`が含まれます。

データを収集するために明示的なユーザー同意が必要な場合、Adobeでは、デフォルトの同意を&#x200B;**[!UICONTROL Out]**&#x200B;または&#x200B;**[!UICONTROL Pending]**&#x200B;のいずれかに設定することをお勧めします。
