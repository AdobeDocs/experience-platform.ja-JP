---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ;スキーマ設計；ミックスイン；ミックスイン；作業の詳細；プロファイル作業；
solution: Experience Platform
title: 勤務先の連絡先の詳細Mixin
topic-legacy: overview
description: このドキュメントでは、作業担当者の詳細ミックスインの概要を説明します。
exl-id: 0133622c-e95f-4833-b2f8-3694d41751b4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# [!UICONTROL 勤務先担当者の] 詳細Mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL [作業連絡先の] 詳細]は、 [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。ミックスインは、勤務先住所、勤務先電子メール、勤務先電話番号、個人が属する組織など、個人に関する職業情報を取得するいくつかのフィールドを提供します。

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
