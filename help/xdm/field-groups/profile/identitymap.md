---
keywords: Experience Platform;ホーム;人気のトピック;スキーマ;XDM;個人プロファイル;フィールド;identityMap;ID マップ;スキーマデザイン;マップ;結合スキーマ;和集合
title: IdentityMap スキーマフィールドグループ
description: XDM Individual Profile クラスについて説明します。
exl-id: c9928e85-ef1e-4739-ba1d-80505a9e60c3
source-git-commit: cfa3e5c6811f148376a2d2012f5687be6fad2299
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 63%

---

# [!UICONTROL IdentityMap] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL IdentityMap] は、[[!UICONTROL XDM ExperienceEvent] クラスの標準スキーマフィールドグループで ](../../classes/experienceevent.md)[[!UICONTROL XDM Individual Profile] クラス ](../../classes/individual-profile.md) の互換性のあるフィールドグループです。 フィールドグループは単一のマップフィールドを提供します。これには、名前空間でキー設定された一連のユーザー ID が含まれます。

![[!UICONTROL IdentityMap] スキーマフィールドグループの図 ](../../images/field-groups/identitymap.png)

メリットとデメリットを含むそのユースケースについては、[スキーマ構成の基本](../../schema/composition.md#identityMap)の ID マップの節を参照してください。

**例**

```JSON
{
    "identityMap":{
        "ECID":[
            {
                "id":"83238819066235616291057085344313877718",
                "authenticatedState":"ambiguous",
                "primary":true
            }
        ]
    }
}
```

フィールドグループについて詳しくは、公開 XDM リポジトリの [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/identitymap.schema.json) を参照してください。
