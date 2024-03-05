---
title: defaultConsent
description: デフォルトの同意収集方法を設定します。
source-git-commit: b9db136a9077e3420bfeb44ae45e7d72c322acbb
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# `defaultConsent`

The `defaultConsent` プロパティは、 [`setConsent`](../setconsent.md) コマンドを使用します。 このプロパティは、データを収集する前に、同意が必要な領域に居住する個人から誤ってデータを収集したくない場合に役立ちます。

このプロパティでは次の 3 つの値を使用できます。

* **In**：データ収集は、ユーザーがオプトアウトするまで、通常どおり実行されます。
* **出力**：ユーザーがオプトインするまで、データは永久に破棄されます。
* **保留中**：ユーザーが [`setConsent`](../setconsent.md) コマンドを使用します。 ページの読み込み間、データは保持されません。

EU 一般データ保護規則 (GDPR) の管轄区域外の訪問者がいる場合は、デフォルトの同意が `in`. GDPR の管轄区域内の訪問者は、デフォルトで、 `pending`. 同意管理プラットフォーム (CMP) は、顧客の地域を検出し、フラグを提供できます `gdprApplies` を IAB TCF 2.0 にアップロードします。このフラグを使用して、デフォルトの同意を設定できます。

## Web SDK タグ拡張を使用したデフォルトの同意の設定

の下で目的のラジオボタンを選択します。 **[!UICONTROL デフォルトの同意]** when [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL プライバシー] 」セクションで、目的のセクションを選択します。 **[!UICONTROL デフォルトの同意]**.
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用したデフォルトの同意の設定

を設定します。 `defaultConsent` 文字列プロパティを `configure` コマンドを使用します。 このプロパティは大文字と小文字を区別し、次の 3 つの値のみをサポートします。 `"in"`, `"out"`、および `"pending"`. 他の値を使用しようとすると、ライブラリによってエラーがスローされます。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "defaultConsent": "pending"
});
```
