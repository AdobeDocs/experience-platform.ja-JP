---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;Schema design;mixin;mixins;work details;profile work;
solution: Experience Platform
title: 作業担当者の詳細ミックスイン
topic: overview
description: このドキュメントでは、作業担当者の詳細ミックスインの概要を説明します。
translation-type: tm+mt
source-git-commit: f9d8021643e72e3fbb5315b54a19815dcdaaa702
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 6%

---


# [!UICONTROL 作業担当者の詳細] Mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、 [mixin名の更新に関するドキュメントを参照してください](../name-updates.md) 。

[!UICONTROL 「Work Contact Details] 」は、 [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。 ミックスインは、勤務先住所、勤務先電子メール、勤務先電話番号、個人が属する組織など、個人に関する職業情報を取得するいくつかのフィールドを提供します。

<img src="../../images/mixins/profile-work-details.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `workAddress` | [住所](../../data-types/postal-address.md) | 個人の勤務先の住所を示します。 |
| `workEmail` | [電子メールアドレス](../../data-types/email-address.md) | ユーザーの勤務先の電子メールアドレスを示します。 |
| `workPhone` | [電話番号](../../data-types/phone-number.md) | その人の勤務先の電話番号を示します。 |
| `organizations` | 文字列（配列） | 個人が属する組織を表す自由形式の文字列の配列です。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-work-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-work-details.schema.json)