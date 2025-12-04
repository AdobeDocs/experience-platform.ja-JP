---
title: Adobe Experience Platform Web SDK拡張機能のアクションタイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なアクションタイプについて説明します。
solution: Experience Platform
exl-id: a4bf0bb9-59b4-4c43-97e6-387768176517
source-git-commit: 19e85ef4dbaeb90712ad9cd6ad4cb9a1a6b0c6a5
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# アクションタイプ

このページには、タグルール内のサポートされるすべてのアクションが一覧表示されます。 タグルール内でアクションを作成または編集するには：

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、目的の [!UICONTROL Action Type] を選択します。

Web SDK タグ拡張機能には、次のアクションがあります。

* [**[!UICONTROL Apply propositions]**](apply-propositions.md)：単一ページアプリケーションで、指標を増分せずに提案をレンダリングします。
* [**[!UICONTROL Apply response]**](apply-response.md):Edge Networkからの応答に基づいてアクションを実行します。
* [**[!UICONTROL Evaluate rulesets]**](evaluate-rulesets.md)：ルールセットの評価を手動でトリガーします。
* [**[!UICONTROL Get Media Analytics tracker]**](get-media-analytics-tracker.md)：従来の Media API をウィンドウオブジェクトに書き出します。
* [**[!UICONTROL Redirect with identity]**](redirect-with-identity.md)：所有しているドメイン間での訪問者識別子の共有を許可します。
* [**[!UICONTROL Send event]**](send-event.md)：イベントデータをEdge Networkに送信します。
* [**[!UICONTROL Send media event]**](send-media-event.md)：メディアデータをEdge Networkに送信します。
* [**[!UICONTROL Set consent]**](set-consent.md)：訪問者に必要な同意を設定します。
* [**[!UICONTROL Update variable]**](update-variable.md): [ 変数 ](../data-element-types.md#variable) データ要素を変更します。
