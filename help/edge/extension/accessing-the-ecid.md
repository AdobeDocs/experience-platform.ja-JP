---
title: 'ECID へのアクセス '
description: Adobe Experience Platform Web SDK拡張機能（タグ内のECIDを活用）
source-git-commit: befe1efa884706165b8d65803d06f6370a8a60f2
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 10%

---


# ECID へのアクセス

[!DNL Experience Cloud Identity (ECID)]は、Webサイトの訪問者の永続的な識別子です。 状況によっては、ECIDにアクセスして（例えばサードパーティに送信するため）、

タグ内のECIDにアクセスするには、Adobeで次の操作をお勧めします。

1. プロパティで、[ルールコンポーネントのシーケンス](../../tags/ui/managing-resources/rules.md#sequencing)が有効になっていることを確認します。
1. 新しいルールを作成します。
1. [!UICONTROL Library Loaded]イベントをルールに追加します。
1. 次のコードを使用して、[!UICONTROL Custom Condition]アクションをルールに追加します（SDKインスタンスに設定した名前は`alloy`とします）。

   ```javascript
    return alloy("getIdentity")
      .then(function(result) {
        _satellite.setVar("ECID", result.identity.ECID);
      });
   ```

1. ルールを保存します。

その後、他のデータ要素と同様に、`%ECID%`または`_satellite.getVar("ECID")`を使用して後続のルールのECIDにアクセスできるようになります。
