---
title: ECID へのアクセス
description: Adobe Experience Platform Web SDK 拡張機能（タグ内の ECID を活用）
exl-id: 8e63a873-d7b5-4c6c-b14d-3c3fbc82b62f
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 10%

---

# ECID へのアクセス

[!DNL Experience Cloud Identity (ECID)] は、Web サイトへの訪問者の永続的な識別子です。 状況によっては、ECID にアクセスして（例えばサードパーティに送信するため）、

タグ内の ECID にアクセスするには、Adobeで次の操作をお勧めします。

1. プロパティで、[ ルールコンポーネントの優先順位 ](../../tags/ui/managing-resources/rules.md#sequencing) が有効になっていることを確認します。
1. 新しいルールを作成します。
1. [!UICONTROL Library Loaded] イベントをルールに追加します。
1. 次のコードを使用して、ルールに [!UICONTROL Custom Condition] アクションを追加します（SDK インスタンスに設定した名前は `alloy` とします）。

   ```javascript
    return alloy("getIdentity")
      .then(function(result) {
        _satellite.setVar("ECID", result.identity.ECID);
      });
   ```

1. ルールを保存します。

その後、他のデータ要素と同様に、`%ECID%` または `_satellite.getVar("ECID")` を使用して、後続のルールで ECID にアクセスできるようになります。
