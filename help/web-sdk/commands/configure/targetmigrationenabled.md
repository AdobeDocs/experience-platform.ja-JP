---
title: targetMigrationEnabled
description: Web SDK がAdobe Target Cookie を読み書きできるようにします。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---

# `targetMigrationEnabled`

The `targetMigrationEnabled` プロパティは、Web SDK がAdobe Target 1.x および 2.x ライブラリが使用する mbox Cookie と mboxEdgeCluster Cookie の読み取りと書き込みを可能にするブール値です。 このオプションを使用すると、以前のAdobe Target実装を使用したページと Web SDK を使用したページの間で訪問者プロファイルを保持できます。

## Web SDK タグ拡張機能を使用した at.js からの Target 移行の有効化

を選択します。 **[!UICONTROL at.js から Web SDK への Target の移行]** チェックボックス [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL パーソナライズ] 「 」セクションで、「 」チェックボックスをオンにします。 **[!UICONTROL at.js から Web SDK への Target の移行]**.
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用した at.js からの Target の移行の有効化

を設定します。 `targetMigrationEnabled` 実行時のブール値 `configure` コマンドを使用します。 Web SDK を設定する際にこのプロパティを省略した場合、デフォルトはになります。 `false`. この値をに設定します。 `true` Adobe Target 1.x または 2.x ライブラリをまだ使用しているページがある場合。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "targetMigrationEnabled": true
});
```
