---
title: ECID へのアクセス
description: Adobe Experience Platform Web SDK 拡張機能（タグでの ECID の活用）
exl-id: 8e63a873-d7b5-4c6c-b14d-3c3fbc82b62f
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 10%

---

# ECID へのアクセス

この [!DNL Experience Cloud Identity (ECID)] は、Web サイトの訪問者の永続的な識別子です。 状況によっては、ECID にアクセスする（例えば、サードパーティに送信する）ことをお勧めします。

タグ内の ECID にアクセスするには、Adobeで次の操作をお勧めします。

1. プロパティがで設定されていることを確認します。 [ルールコンポーネントの順番](../../tags/ui/managing-resources/rules.md#sequencing) 有効。
1. 新しいルールを作成します。
1. を追加します。 [!UICONTROL Library Loaded] イベントをルールに追加します。
1. を追加します。 [!UICONTROL カスタム条件] 次のコードを使用してルールにアクションを追加します（SDK インスタンスに設定した名前がの場合）。 `alloy`):

   ```javascript
    return alloy("getIdentity")
      .then(function(result) {
        _satellite.setVar("ECID", result.identity.ECID);
      });
   ```

1. ルールを保存します。

その後、を使用して後続のルールで ECID にアクセスできるようになります。 `%ECID%` または `_satellite.getVar("ECID")` 他のデータ要素と同様に
