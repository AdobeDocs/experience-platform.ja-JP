---
title: ID 設定
description: タグ拡張機能で訪問者を識別する方法を定義します。
source-git-commit: 217282135bcd750740f4d3f8c6e17a0b8f9578bd
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 2%

---

# ID 設定

この設定セクションを使用すると、ユーザー ID の処理に関する web SDKの動作を定義できます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Identity]** セクションまで下にスクロールします。

![&#x200B; タグ UI の Web SDK タグ拡張機能の ID 設定を示す画像 &#x200B;](../assets/web-sdk-ext-identity.png)

次のオプションがあります。

## [!UICONTROL Migrate ECID from VisitorAPI]

Web SDKが `AMCV` および `s_ecid` Cookie を読み取り、`AMCV` が使用する `Visitor.js` Cookie を設定できるチェックボックス。 この機能は、`VisitorAPI.js` を使用するライブラリから Web SDKに移行する際に重要です。一部のページでは引き続き `Visitor.js` を使用している可能性があるからです。 このオプションを使用すると、SDKは引き続き同じ ECID を使用できるので、ユーザーが 2 人の異なるユーザーとして識別されることはありません。 このチェックボックスに相当するJavaScript ライブラリは [`idMigrationEnabled`](/help/collection/js/commands/configure/idmigrationenabled.md) です。

## [!UICONTROL Use third-party cookies]

このオプションが有効な場合、Web SDKはユーザー ID をサードパーティ Cookie に格納しようとします。 成功した場合、ユーザーは、各ドメインで個別のユーザーとして識別されるのではなく、複数のドメインを移動する際に単一のユーザーとして識別されます。 このオプションが有効になっている場合、ブラウザーがサードパーティ cookie をサポートしていない場合や、ユーザーによってサードパーティ cookie が許可されないように設定されている場合には、SDKでサードパーティ cookie にユーザー ID を格納できない可能性があります。 この場合、SDKはファーストパーティドメインにのみ ID を保存します。 このチェックボックスに相当するJavaScript ライブラリは [`thirdPartyCookiesEnabled`](/help/collection/js/commands/configure/thirdpartycookiesenabled.md) です。

>[!IMPORTANT]
>
>サードパーティ cookie は、Web SDKの [&#x200B; ファーストパーティデバイス ID](/help/collection/use-cases/identity/first-party-device-ids.md) 機能と互換性がありません。 ファーストパーティデバイス ID を使用するか、サードパーティ Cookie を使用することができます。両方の機能を同時に使用することはできません。
