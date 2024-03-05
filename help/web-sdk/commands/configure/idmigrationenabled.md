---
title: idMigrationEnabled
description: Web SDK が AMCV Cookie を読み取ることを許可します。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# `idMigrationEnabled`

The `idMigrationEnabled` プロパティを使用すると、Web SDK は以前のAdobe Experience Cloud実装で設定された AMCV Cookie を読み取ることができます。 組織で実装を Web SDK にアップグレードした場合、この設定により、現在のAdobe Experience Cloud ID サービスへの移行をよりスムーズにおこなうことができます。 この設定は、Web SDK へのアップグレード時に個別訪問者数が大幅に増加するのを防ぐために役立ちます。

組織で Web SDK の新規実装を実行している場合、この設定を有効にしても、データ収集や訪問者の識別には影響しません。 すべての実装で有効のままにしておくという欠点はありません。

## Web SDK タグ拡張機能を使用した ID 移行の有効化

を選択します。 **[!UICONTROL VisitorAPI から Web SDK への ECID の移行]** チェックボックス [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 次を見つけます。 [!UICONTROL ID] 「 」セクションで、「 」チェックボックスをオンにします。 **[!UICONTROL VisitorAPI から Web SDK への ECID の移行]**.
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用した ID 移行の有効化

を設定します。 `idMigrationEnabled` 実行時のブール値 `configure` コマンドを使用します。 Web SDK を設定する際にこのプロパティを省略した場合、デフォルトはになります。 `true`. 訪問者 API で設定された AMCV Cookie を読み取る機能を無効にする場合は、このプロパティを設定します。 ほとんどの組織では、このプロパティを設定する必要はありません。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "idMigrationEnabled": false
});
```
