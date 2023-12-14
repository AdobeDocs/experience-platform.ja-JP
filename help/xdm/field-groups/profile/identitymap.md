---
keywords: Experience Platform;ホーム;人気のトピック;スキーマ;XDM;個人プロファイル;フィールド;identityMap;ID マップ;スキーマデザイン;マップ;結合スキーマ;和集合
title: IdentityMap スキーマフィールドグループ
description: このドキュメントでは、XDM 個人プロファイルクラスの概要を説明します。
exl-id: c9928e85-ef1e-4739-ba1d-80505a9e60c3
source-git-commit: 43b3b79a4d24fd92c7afbf9ca9c83b0cbf80e2c2
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 81%

---

# [!UICONTROL IdentityMap] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL IdentityMap] は、[[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md)の標準スキーマフィールドグループです。 フィールドグループは単一のマップフィールドを提供します。これには、名前空間でキー設定された一連のユーザー ID が含まれます。

![の図 [!UICONTROL IdentityMap] スキーマフィールドグループ](../../images/field-groups/identitymap.png)

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

フィールドグループについて詳しくは、 [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/identitymap.schema.json) パブリック XDM リポジトリー内に保存されます。
