---
title: Experience Cloud IDの取得
seo-title: Adobe Experience Platform Web SDK Experience Cloud IDの取得
description: Adobe Experience Cloud IDを取得する方法を説明します。
seo-description: Adobe Experience Cloud IDを取得する方法を説明します。
translation-type: tm+mt
source-git-commit: fc48c11cb1f8a88f9fec8a36646f59f39ac3819f
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 19%

---


# （ベータ版）Experience Cloud IDの取得

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

Adobe Experience Cloudでは、各消費者に対して一意のIDが使用されます。 この一意のIDを使用する場合は、 `getIdentity` コマンドを使用します。 `getIdentity` 現在の訪問者の既存のECIDを返します。 ECIDをまだ持っていない初回訪問者の場合は、新しいECIDが生成されます。

>[!NOTE]
>
>この方法は、通常、Experience Cloud IDを読み取る必要があるカスタムソリューションで使用されます。 標準の実装では使用されません。

```javascript
alloy("getIdentity")
  .then(function(ecid) {
    // This function will get called with Adobe Experience Cloud Id when the command promise is resolved
  })
  .catch(function(error) {
    // The command failed.
    // "error" will be an error object with additional information
  })
```
