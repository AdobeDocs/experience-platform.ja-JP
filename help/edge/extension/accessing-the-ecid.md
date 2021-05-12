---
title: 'ECIDへのアクセス '
description: Adobe Experience Platform LaunchのECIDを活用したAdobe Experience PlatformWeb SDK拡張機能
source-git-commit: 3002036d7366e2f7310aa62e53c7c391d9ff7a07
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 5%

---


# ECIDへのアクセス

[!DNL Experience Cloud Identity (ECID)]は、Webサイトへの訪問者の永続的な識別子です。 場合によっては、（例えばサードパーティに送信するために）ECIDにアクセスすることをお勧めします。

Adobeは、Adobe Experience Platform Launch内のECIDにアクセスする場合、以下の操作を推奨します。

1. プロパティが[ルールコンポーネントシーケンス](https://experienceleague.adobe.com/docs/launch/using/ui/rules.html?lang=en#rule-component-sequencing)が有効に設定されていることを確認します。
1. 新しいルールを作成します。
1. ルールに対する追加[!UICONTROL ライブラリの読み込み済み]イベントです。
1. 次のコード追加を持つルールに対する[!UICONTROL カスタム条件]アクション（SDKインスタンス用に設定した名前が`alloy`であると仮定）:

   ```javascript
    return alloy("getIdentity")
      .then(function(result) {
        _satellite.setVar("ECID", result.identity.ECID);
      });
   ```

1. ルールを保存します。

その後、他のデータ要素と同様に、`%ECID%`や`_satellite.getVar("ECID")`を使用して、後続のルールのECIDにアクセスできるようになります。
