---
title: Media Analytics トラッカーを取得
description: ウィンドウオブジェクトにレガシーメディア API を書き出します。
source-git-commit: c55e425f146e4afdb2314b432c9dc48391e02e63
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 2%

---

# Media Analytics トラッカーを取得

**[!UICONTROL Get Media Analytics tracker]** アクションは、従来の Media Analytics API を取得するために使用されます。 アクションを設定してオブジェクト名を指定すると、従来の Media Analytics API がそのウィンドウオブジェクトに書き出されます。 このアクションは、従来の Media Analytics からストリーミングメディア分析に移行する場合に役立ちます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Get Media Analytics tracker]** に設定します。

![Media Analytics トラッカーの取得アクションタイプを示すExperience Platform UI 画像。](../assets/get-media-analytics-tracker.png)

このアクションには、設定可能な単一のフィールドが含まれています。

* **[!UICONTROL Export the Media Legacy API to this window object]**: Media Legacy API を書き出す対象のオブジェクトを選択します。 何も指定されない場合、アクションは API を `window.Media` に書き出します。
