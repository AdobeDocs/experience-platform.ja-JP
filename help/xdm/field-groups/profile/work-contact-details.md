---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；mixin;mixin；作業の詳細；プロファイル作業；
solution: Experience Platform
title: 仕事用連絡先詳細スキーマフィールドグループ
description: このドキュメントでは、「仕事用連絡先詳細」スキーマフィールドグループの概要を説明します。
exl-id: 0133622c-e95f-4833-b2f8-3694d41751b4
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 15%

---


# [!UICONTROL 仕事用連絡先の詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 仕事用連絡先の詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md). 「 」フィールドグループには、個人に関する職業情報をキャプチャするためのフィールドが複数用意されています。例えば、勤務先住所、勤務先 E メール、勤務先電話番号、個人が属する組織などです。

![](../../images/field-groups/work-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `workAddress` | [住所](../../data-types/postal-address.md) | 人物の住所を表します。 |
| `workEmail` | [メールアドレス](../../data-types/email-address.md) | 人物の仕事用電子メールアドレスを表します。 |
| `workPhone` | [電話番号](../../data-types/phone-number.md) | 人物の勤務先電話番号を表します。 |
| `organizations` | 文字列（配列） | 人物が属する組織を表す自由形式の文字列の配列。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.schema.json)
