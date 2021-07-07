---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；mixin;mixin；作業の詳細；プロファイル作業；
solution: Experience Platform
title: 勤務先詳細スキーマフィールドグループ
topic-legacy: overview
description: This document provides an overview of the Work Contact Details schema field group.
exl-id: 0133622c-e95f-4833-b2f8-3694d41751b4
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 6%

---


# [!UICONTROL 「勤務先詳細スキ] ーマ」フィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 See the document on [field group name updates](../name-updates.md) for more information.

[!UICONTROL 作業用連絡先] クラスの標準スキーマフィールドグル [[!DNL XDM Individual Profile] ープ](../../classes/individual-profile.md)。The field group provides several fields that capture occupational information regarding an individual person, such as work address, work email, work phone number, and organizations to which the person belongs.

![](../../images/field-groups/work-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `workAddress` | [住所](../../data-types/postal-address.md) | 個人の勤務先住所を示します。 |
| `workEmail` | [電子メールアドレス](../../data-types/email-address.md) | 個人の勤務先の電子メールアドレスを示します。 |
| `workPhone` | [電話番号](../../data-types/phone-number.md) | Describes the person&#39;s work phone number. |
| `organizations` | String (Array) | An array of free-form strings that represent the organizations the person is a member of. |

{style=&quot;table-layout:auto&quot;}

For more details on the field group, refer to the public XDM repository:

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.example.1.json)
* [Full schema](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.schema.json)
