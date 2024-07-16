---
title: edgeBasePath
description: Adobe サービスの操作に使用するエンドポイントのベースパス。
exl-id: 3542575d-ad02-415c-8e47-97c877dfdd9d
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# `edgeBasePath`

Adobe サービスを操作する際、`edgeBasePath` プロパティは対象のパスを変更します。 ほとんどの組織では、このプロパティを設定または変更する必要はありません。

## Web SDK タグ拡張機能を使用したEdgeのベースパス

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL Edgeのベースパス]**」テキストフィールドに目的のテキストを入力します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 ] をクリックします。
1. 「[!UICONTROL  詳細設定 ]」セクションまでスクロールし、「**[!UICONTROL Edgeのベースパス]**」テキストフィールドに目的の値を入力します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用したEdgeのベースパス

`configure` コマンドの実行時に「`edgeBasePath`」テキストフィールドを設定します。 このプロパティを省略すると、既定値の `ee` が使用されます。 Adobeでは、ほとんどの設定でこのプロパティを省略することをお勧めします。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "edgeBasePath": "ee"
});
```
