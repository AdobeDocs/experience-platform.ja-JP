---
title: idMigrationEnabled
description: Web SDK が AMCV Cookie を読み取れるようにします。
exl-id: 33b9d645-0fbe-4fe4-8847-e6f9e78557b6
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# `idMigrationEnabled`

`idMigrationEnabled` プロパティを使用すると、Web SDK で以前のAdobe Experience Cloud実装によって設定された AMCV Cookie を読み取ることができます。 実装を Web SDK にアップグレードする場合、この設定を使用すると、現在のAdobe Experience Cloud ID サービスへの移行をよりスムーズにおこなうことができます。 この設定は、Web SDK へのアップグレード時にユニーク訪問者が急増しないように役立ちます。

組織で新しい Web SDK 実装を実行する場合、この設定を有効にしても、データ収集や訪問者の識別には影響しません。 すべての実装で有効にしておいても欠点はありません。

## Web SDK タグ拡張機能を使用して ID 移行を有効にする

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL VisitorAPI から Web SDK に ECID を移行]**」チェックボックスを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL ID]」セクションを見つけ、「**[!UICONTROL VisitorAPI から web SDK に ECID を移行]**」チェックボックスを選択します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用して ID 移行を有効にする

`configure` コマンドを実行するときは、`idMigrationEnabled` のブール値を設定します。 Web SDK の設定時にこのプロパティを省略すると、デフォルトは `true` になります。 訪問者 API で設定された AMCV Cookie の読み取り機能を無効にする場合は、このプロパティを設定します。 ほとんどの組織では、このプロパティを設定する必要はありません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  idMigrationEnabled: false
});
```
