---
title: eventType
description: sendEvent 呼び出しのイベントのタイプを設定します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# `eventType`

The `eventType` プロパティを使用すると、Web SDK を使用して送信するイベントのタイプを定義できます。 このフィールドには最終的に `xdm.eventType` フィールドに入力します。 Adobeに送信するイベントタイプを区別する場合に役立ちます。

Adobeには、使用できる事前定義済みのイベントタイプがいくつか用意されています。 詳しくは、 [次の値を使用できます： `eventType`](/help/xdm/classes/experienceevent.md#accepted-values-for-eventtype) （XDM ユーザーガイド）の事前定義済み値の完全なリストを参照してください。 必要に応じて、独自の値を使用することもできます。

次の両方を設定した場合、 `type` ここで `xdm.eventType` （内） [`xdm`](xdm.md) オブジェクトの場合、このフィールドの値が優先されます。

## Web SDK タグ拡張機能を使用したイベントタイプの設定

を設定します。 **[!UICONTROL タイプ]** タグルールのアクション内のドロップダウンフィールド。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. 次の項目の下のドロップダウンを使用します。 **[!UICONTROL タイプ]** フィールドに値を入力するか、独自の値を入力します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したイベントタイプの設定

を設定します。 `eventType` 文字列プロパティ（実行時） `sendEvent` コマンドを使用します。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "type": "commerce.purchases"
});
```
