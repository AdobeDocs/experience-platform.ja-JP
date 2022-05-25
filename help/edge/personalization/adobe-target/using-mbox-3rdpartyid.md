---
title: mbox3rdPartyId のリアルタイムプロファイル同期
description: Adobe Experience Platform Web SDK で mbox3rdPartyId を使用する方法について説明します。
keywords: パーソナライゼーション；target;adobe target;renderDecisions;sendEvent;mbox3rdPartyId;
exl-id: 677d1054-0769-4ec6-811e-e02d4b247c2a
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 9%

---

# `mbox3rdPartyId` について

Adobe Targetの mbox3rdPartyId は、会社のロイヤルティプログラムのメンバーシップ ID など、会社の訪問者 ID です。

訪問者が会社のサイトにログインすると、通常、会社は訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社で適用可能なその他の識別子に結び付けられる ID を作成します。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=en#)


## 使用方法 `mbox3rdPartyId` Web SDK を使用

### 手順 1:の設定 `Target Third Party ID Namespace`

の設定 `Target Third Party ID Namespace` の [Datastream](../../datastreams/overview.md)を使用し、mbox サードパーティ ID として使用する ID 名前空間を使用します。
[ID 名前空間の詳細を説明します](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)

![](assets/mbox3rdpartyid.png)

### 手順 2:を `mbox3rdpartyId` ターゲットに

を `mbox3rdpartyId` を `sendEvent` 」コマンドを使用します。手順 1 で設定した ID 名前空間を使用します。
[ID の送信の詳細](../../identity/overview.md#syncing-identities)

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
