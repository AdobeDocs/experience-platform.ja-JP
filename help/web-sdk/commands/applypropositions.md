---
title: applyPropositions
description: sendEvent を使用して既にレンダリングされている提案を再レンダリングします。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---


# `applyPropositions`

The `applyPropositions` コマンドを使用すると、既にレンダリングされている提案を、 [`sendEvent`](sendevent/overview.md) コマンドを使用します。 このコマンドは、ページの一部が再レンダリングされ、既にページに適用されているペルソナライゼーションが上書きされる可能性があるシングルページアプリケーションを操作する場合に役立ちます。

このコマンドは、次のフィールドをサポートします。

* **提案**：再レンダリングする提案オブジェクトの配列。
* **ビュー名**：レンダリングするビューの名前。 これらの決定の表示通知はキャッシュされ、後続の `sendEvent` コマンドを使用する `personalization.includePendingDisplayNotifications` オプション。
* **メタデータ**:HTMLオファーの適用方法を決定するオブジェクト。 次のプロパティが含まれます。
   * 範囲
   * セレクター
   * アクションタイプ

## Web SDK タグ拡張機能を使用した提案の適用

提案の適用は、Adobe Experience Platformのデータ収集タグインターフェイスのルール内でアクションとして実行されます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL 提案を適用]**.
1. 右側の目的のフィールドを設定します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用した提案の適用

を実行します。 `applyPropositions` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 設定オプションを含むオブジェクトは、次のフィールドをサポートします。

* **`propositions`**：再レンダリングする提案オブジェクトの配列。 このオブジェクトは、通常、 `propositionScopes` 通常、フィールドは、再レンダリングするスコープまたはサーフェスを決定します。
* **`metadata`**:HTMLオファーの適用方法を決定します。 これは、キーがスコープまたはサーフェスであるマップで、値はキーを含むオブジェクトです `selector` および `actionType`.
   * `selector`:HTMLを適用する場所の CSS セレクターを含む文字列。
   * `actionType`:HTMLで実行するアクション。 有効な値には、`setHtml`、`replaceHtml`、`appendHtml` などがあります。
* **`viewName`**：単一ページアプリケーションでレンダリングするビューの名前。 これらの決定の表示通知はキャッシュされ、後続の `sendEvent` コマンドを使用 `personalization.includePendingDisplayNotifications`.

```js
alloy("applyPropositiions",{
  "propositions": [],
  "metadata": {},
  "viewName": ""
});
```
