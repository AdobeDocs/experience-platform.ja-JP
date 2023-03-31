---
title: ECID へのアクセス
description: Adobe Experience PlatformタグでExperience CloudID(ECID) にアクセスする方法を説明します
exl-id: 8e63a873-d7b5-4c6c-b14d-3c3fbc82b62f
source-git-commit: db7700d5c504e484f9571bbb82ff096497d0c96e
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 9%

---


# ECID へのアクセス

この [!DNL Experience Cloud ID (ECID)] は、Web サイトのExperience Cloudを識別するのに役立つ永続的な訪問者識別子です。 識別子をサードパーティプラットフォームに送信するなど、特定の状況では、 [!DNL ECID].

次の手順で [!DNL ECID] タグ内で、次の手順に従います。

1. プロパティがで設定されていることを確認します。 [ルールコンポーネントの順番](../../tags/ui/managing-resources/rules.md#sequencing) 有効。
2. 新しいルールを作成します。
3. を追加します。 [!UICONTROL Library Loaded] イベントをルールに追加します。
4. を追加します。 [!UICONTROL カスタム条件] 次のコードを使用して、ルールに対するアクションを作成します（SDK インスタンスに設定した名前がの場合）。 `alloy`):

   ```javascript
   return alloy("getIdentity")
       .then(function(result) {
           _satellite.setVar("ECID", result.identity.ECID);
       });
   ```

5. ルールを保存します。

これで、 [!DNL ECID] 後続のルールで、を使用 `%ECID%` または `_satellite.getVar("ECID")`（他のデータ要素にアクセスする方法と同様）
