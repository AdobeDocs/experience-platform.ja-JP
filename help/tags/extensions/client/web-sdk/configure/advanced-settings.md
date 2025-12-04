---
title: 詳細設定
description: Web SDK タグ拡張機能の詳細設定を行います。
source-git-commit: d6aea91d6989775ff5b6038b216ed2518f4a7d98
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 2%

---

# 詳細設定

この設定セクションでは、詳細設定を変更できます。 Adobeでは、ほとんどの実装で、これらのオプションをそのままにしておくことをお勧めします。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Advanced Settings]** セクションまで下にスクロールします。

![Web SDK タグ拡張機能ページを使用した詳細設定を示す画像 ](../assets/advanced-settings.png)

現在、利用可能なオプションが 1 つあります。

## [!UICONTROL Edge base path]

このフィールドを使用して、Edge Networkとのやり取りに使用するベースパスを変更します。 AdobeAdobeは、特定のアルファまたはベータのテストに参加する場合、このフィールドの変更をリクエストすることがあります。それ以外の場合は、デフォルト値の `ee` のままにすることをお勧めします。

このフィールドは、JavaScript ライブラリを設定する際のタグの [`edgeBasePath`](/help/collection/js/commands/configure/edgebasepath.md) に相当します。
