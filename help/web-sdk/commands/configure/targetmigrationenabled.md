---
title: targetMigrationEnabled
description: Web SDK がAdobe Target Cookie の読み取りおよび書き込みを行えるようにします。
exl-id: 4b9203c6-31b7-45af-a6a6-a206d7edac3f
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---

# `targetMigrationEnabled`

`targetMigrationEnabled` プロパティは、Adobe Target 1.x および 2.x ライブラリが使用する mbox および mboxEdgeCluster Cookie の読み取りと書き込みを Web SDK に許可するブール値です。 このオプションを使用すると、以前のAdobe Target実装を使用したページと、Web SDK を使用したページとの間で訪問者プロファイルを保持できます。

## Web SDK タグ拡張機能を使用して at.js から Target の移行を有効にする

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL at.js から Web SDK にターゲットを移行]**」チェックボックスを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL Personalization]」セクションまでスクロールし、「**[!UICONTROL Target を at.js から Web SDK に移行]**」チェックボックスをオンにします。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用して、at.js から Target の移行を有効にします

`configure` コマンドを実行するときは、`targetMigrationEnabled` のブール値を設定します。 Web SDK の設定時にこのプロパティを省略すると、デフォルトは `false` になります。 一部のページでAdobe Target 1.x または 2.x ライブラリが使用されている場合は、この値を `true` に設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  targetMigrationEnabled: true
});
```
