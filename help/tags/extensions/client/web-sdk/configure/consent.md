---
title: 同意設定
description: タグ拡張機能のデフォルトの同意およびプライバシー設定を指定します。
exl-id: 93913a8b-0351-409d-b26a-8dc2ac0296c5
source-git-commit: 6c05d8abde0e4d6b07fe37d6e3eacd5d3dd67ec2
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 1%

---

# 同意設定 {#consent}

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_consent"
>title="同意"
>abstract="他の明示的な同意設定が指定されていない場合に想定されるデフォルトの同意レベルを選択します。"

**[!UICONTROL Consent]** のセクションでは、他の明示的な同意環境設定が指定されていない場合に想定されるデフォルトの同意レベルを選択できます。 デフォルトの同意レベルがユーザープロファイルに保存されない。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Consent]** セクションまで下にスクロールします。

![&#x200B; タグ UI の web SDK タグ拡張機能のプライバシー設定を示す画像 &#x200B;](../assets/web-sdk-ext-privacy.png)

このセクションには、デフォルトの同意レベルを決定する単一のラジオボタンセットが含まれています。

* **[!UICONTROL In]**：ユーザーが同意環境設定を指定する前に発生するイベントを収集します。
* **[!UICONTROL Out]**：ユーザーが同意環境設定を指定する前に発生するイベントをドロップします。
* **[!UICONTROL Pending]**：ユーザーが同意環境設定を指定する前に発生するイベントをキューに追加します。 同意が得られると、キュー内のイベントがAdobeに送信されます。 同意が拒否されると、キュー内のイベントは破棄されます。
* **[!UICONTROL Provide a data element]**：上記のいずれかの設定を決定するデータ要素を選択します。 有効な値には、文字列 `"in"`、`"out"`、`"pending"` があります。

データを収集するために明示的なユーザー同意が必要な場合、Adobeでは、デフォルトの同意を **[!UICONTROL Out]** または **[!UICONTROL Pending]** に設定することをお勧めします。
