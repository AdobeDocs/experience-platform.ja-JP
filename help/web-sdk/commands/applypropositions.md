---
title: applyPropositions
description: sendEvent で既にレンダリングされている提案を再レンダリングします。
exl-id: 6b79f334-4ea6-4ba4-8640-d35b7f90df98
source-git-commit: 4c7313afdce6645ab638b2998573e5a4f7c5de8f
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# `applyPropositions`

`applyPropositions` コマンドを使用すると、[`sendEvent`](sendevent/overview.md) コマンドを使用して既にレンダリングされた提案を再レンダリングできます。 このコマンドは、単一ページアプリケーションで作業しているときにページの一部が再レンダリングされると、ページに適用済みのパーソナライゼーションが上書きされる可能性があるので便利です。

このコマンドは、次のフィールドをサポートしています。

* **提案**：再レンダリングする提案オブジェクトの配列。
* **ビュー名**: レンダリングするビューの名前。 これらの決定に対する表示通知はキャッシュされ、`personalization.includeRenderedPropositions` オプションを使用して、後続の `sendEvent` コマンドに含めることができます。
* **メタデータ**:HTMLオファーの適用方法を指定するオブジェクト。 これには、次のプロパティが含まれます。
   * 範囲
   * セレクター
   * アクションタイプ

## Web SDK タグ拡張機能を使用した提案の適用

提案の適用は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL &#x200B; 拡張機能 &#x200B;] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL 提案を適用]** に設定します。
1. 目的のフィールドを右側に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用して提案を適用します

設定済みの Web SDK インスタンスを呼び出す際に、`applyPropositions` コマンドを実行します。 設定オプションを含むオブジェクトは、次のフィールドをサポートしています。

* **`propositions`**：再レンダリングする提案オブジェクトの配列。 `propositionScopes` フィールドは通常、どのスコープまたはサーフェスを再レンダリングするかを決定するので、通常、このオブジェクトは使用されません。
* **`metadata`**:HTMLオファーの適用方法を決定します。 キーがスコープまたはサーフェスであり、値がキー `selector` と `actionType` を含むオブジェクトであるマップです。
   * `selector`:HTMLの適用先の CSS セレクターを含む文字列
   * `actionType`:HTMLで実行するアクション。 有効な値には、`setHtml`、`replaceHtml`、`appendHtml` などがあります。
* **`viewName`**：単一ページアプリケーションでレンダリングするビューの名前。 これらの決定に対する表示通知はキャッシュされ、`personalization.includeRenderedPropositions` を使用して後続の `sendEvent` コマンドに含めることができます。

```js
alloy("applyPropositions",{
  "propositions": [],
  "metadata": {},
  "viewName": ""
});
```
