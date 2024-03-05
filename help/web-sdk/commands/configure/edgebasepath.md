---
title: edgeBasePath
description: エンドポイントとのやり取りに使用するAdobe サービスのベースパス。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# `edgeBasePath`

The `edgeBasePath` プロパティを使用すると、宛先のパスが変更されます (Adobe サービス)。 ほとんどの組織では、このプロパティを設定または変更する必要はありません。

## Web SDK タグ拡張機能を使用したエッジのベースパス

目的のテキストを **[!UICONTROL エッジの基本パス]** 次の場合のテキストフィールド [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL 詳細設定] 」セクションで、目的の値を **[!UICONTROL エッジの基本パス]** テキストフィールド。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用したエッジのベースパス

を設定します。 `edgeBasePath` 実行時のテキストフィールド `configure` コマンドを使用します。 このプロパティを省略した場合、デフォルト値は `ee`. Adobeでは、ほとんどの設定からこのプロパティを省略することをお勧めします。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "edgeBasePath": "ee"
});
```
