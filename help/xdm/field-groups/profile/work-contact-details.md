---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；mixin;mixin；作業の詳細；プロファイル作業；
solution: Experience Platform
title: 勤務先詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「勤務先詳細」スキーマフィールドグループの概要を説明します。
exl-id: 0133622c-e95f-4833-b2f8-3694d41751b4
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 6%

---


# [!UICONTROL 「勤務先詳細スキ] ーマ」フィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL 作業用連絡先] クラスの標準スキーマフィールドグル [[!DNL XDM Individual Profile] ープの詳細](../../classes/individual-profile.md)。フィールドグループは、勤務先住所、勤務先 E メール、勤務先電話番号、個人が所属する組織など、個人に関する職業情報を取り込むためのフィールドを複数提供します。

![](../../images/field-groups/work-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `workAddress` | [住所](../../data-types/postal-address.md) | 個人の勤務先住所を示します。 |
| `workEmail` | [電子メールアドレス](../../data-types/email-address.md) | 個人の勤務先の電子メールアドレスを示します。 |
| `workPhone` | [電話番号](../../data-types/phone-number.md) | 個人の勤務先の電話番号を示します。 |
| `organizations` | 文字列（配列） | 個人が属する組織を表す自由形式の文字列の配列です。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.schema.json)
