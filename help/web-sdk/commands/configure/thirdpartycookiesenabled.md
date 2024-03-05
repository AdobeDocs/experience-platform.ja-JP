---
title: thirdPartyCookiesEnabled
description: サードパーティ Cookie を使用して訪問者を識別することを許可する。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# `thirdPartyCookiesEnabled`

The `thirdPartyCookiesEnabled` プロパティは、Web SDK が Cookie をサードパーティのコンテキストに設定するかどうかを決定するブール値です。 このオプションを有効にすると、組織が所有するサブドメインまたはドメイン間の訪問者を識別する場合に役立ちます。 ただし、最新のブラウザーの多くは、サードパーティ Cookie の設定と有効期限を制限しています。

このオプションを有効にすると、Web SDK はAdobe Audience Managerを使用して訪問者を識別します。 このオプションが無効の場合、Audience Managerの呼び出しは無効になります。 詳しくは、 [Demdex ドメインの呼び出しについて](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/demdex-calls.html?lang=ja) (『Audience Managerユーザガイド』) を参照してください。

## Web SDK タグ拡張機能を使用してサードパーティ Cookie を有効にする

を選択します。 **[!UICONTROL サードパーティ Cookie の使用]** チェックボックス [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL ID] 「 」セクションで、「 」チェックボックスをオンにします。 **[!UICONTROL サードパーティ Cookie の使用]**.
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用してサードパーティ Cookie を有効にする

を設定します。 `thirdPartyCookiesEnabled` 実行時のブール値 `configure` コマンドを使用します。 Web SDK を設定する際にこのプロパティを省略した場合、デフォルトはになります。 `true`. この値をに設定します。 `false` Web SDK で訪問者の識別にAudience Managerを使用しない場合。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "thirdPartyCookiesEnabled": false
});
```
