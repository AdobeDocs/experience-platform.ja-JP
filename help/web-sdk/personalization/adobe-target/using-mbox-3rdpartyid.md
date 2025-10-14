---
title: mbox3rdPartyId のリアルタイムプロファイル同期
description: Adobe Experience Platform web SDKで mbox3rdPartyId を使用する方法を説明します。
keywords: パーソナライゼーション；Target;Adobe Target;renderDecisions;sendEvent;mbox3rdPartyId;
exl-id: 677d1054-0769-4ec6-811e-e02d4b247c2a
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 1%

---

# `mbox3rdPartyId` とは

Adobe Targetの mbox3rdPartyId は、会社のロイヤルティプログラムのメンバーシップ ID などの、会社の訪問者 ID です。

訪問者が会社のサイトにログインすると、会社は通常、訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社のその他の該当する識別子に関連付けられた ID を作成します。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=ja#)


## Web SDKでの `mbox3rdPartyId` の使用方法

### 手順 1:`Target Third Party ID Namespace` を設定する

mbox サードパーティ ID として使用する ID 名前空間を使用して、[&#x200B; データストリーム &#x200B;](../../../datastreams/overview.md) の `Target Third Party ID Namespace` を設定します。
[ID 名前空間の詳細情報 &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)

![Target サードパーティ ID 名前空間フィールドを示すExperience Platform UI。](assets/mbox3rdpartyid.png)

### 手順 2:`mbox3rdpartyId` を Target に送信する

手順 1 で設定した ID 名前空間を使用して、`sendEvent` コマンドで Target に `mbox3rdpartyId` を送信します。
[ID の送信の詳細情報 &#x200B;](../../identity/overview.md#syncing-identities)

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Replace `ID_NAMESPACE` with the namespace you have configured in Step 1.
        {
          "id": "1234",
          "authenticatedState": "authenticated"
        }
      ]
    }
  }
});
```
