---
title: ルールセットの評価
description: ルールセットの評価を手動でトリガーします。
source-git-commit: 46e5d007b27eaa67c9ee49e35a711424de383d68
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# ルールセットの評価

「**[!UICONTROL Evaluate rulesets]**」アクションタイプを使用すると、ルールセットのトリガーを手動で設定できます。 ルールセットはAdobe Journey Optimizerから返され、ブラウザー内メッセージなどの機能をサポートします。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Evaluate rulesets]** に設定します。

![ 「ルールセットを評価」応答アクションタイプを示すExperience Platform ユーザーインターフェイスの画像。](../assets/evaluate-rulesets.png)

## 使用可能なフィールド

このアクションタイプは、次のオプションをサポートしています。

* **[!UICONTROL Render visual personalization decisions]**：有効な場合に、一致するルールセット項目のビジュアルパーソナライゼーションの決定をレンダリングするチェックボックス。
* **[!UICONTROL Decision context]**：オンデバイス判定のAdobe Journey Optimizer ルールセットを評価する際に使用されるキー値マップ。 決定コンテキストは手動で、またはデータ要素を通じて提供できます。
