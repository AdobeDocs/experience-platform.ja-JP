---
title: eventType
description: sendEvent 呼び出しのイベントのタイプを設定します。
exl-id: 9d0fae3b-827a-4084-b460-b755e478e06a
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# `eventType`

`eventType` プロパティを使用すると、Web SDK を使用して送信するイベントのタイプを定義できます。 このフィールドは最終的に「`xdm.eventType`」フィールドに入力されます。 Adobeに送信するイベントタイプを区別する際に役立ちます。

Adobeには、使用可能な事前定義済みのイベントタイプがいくつか用意されています。 事前定義済みの値の完全なリストについては、XDM ユーザーガイドの [`eventType`](/help/xdm/classes/experienceevent.md#accepted-values-for-eventtype) に使用できる値」を参照してください。 必要に応じて、独自の値を使用することもできます。

[`xdm`](xdm.md) オブジェクトで `type` と `xdm.eventType` の両方を設定した場合は、このフィールドの値が優先されます。

## Web SDK タグ拡張機能を使用したイベントタイプの設定

タグルールのアクション内の **[!UICONTROL タイプ]** ドロップダウンフィールドを設定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  アクション ] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL  拡張機能 ]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL  アクションタイプ ] を **[!UICONTROL イベントを送信]** に設定します。
1. 「**[!UICONTROL タイプ]**」フィールドのドロップダウンを使用するか、独自の値を入力します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したイベントタイプの設定

`sendEvent` コマンドを実行するときは、`eventType` string プロパティを設定します。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "type": "commerce.purchases"
});
```
