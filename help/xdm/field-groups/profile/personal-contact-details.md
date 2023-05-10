---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；個人の詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 個人の連絡先詳細スキーマフィールドグループ
description: このドキュメントでは、「個人の連絡先詳細」スキーマフィールドグループの概要を説明します。
exl-id: a78d9aee-ecf6-45a9-b270-cdad5b800a86
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 20%

---


# [!UICONTROL 個人の連絡先の詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 個人の連絡先の詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) 個人の連絡先情報を表します。

![](../../images/field-groups/personal-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `faxPhone` | [電話番号](../../data-types/phone-number.md) | 人物の FAX 番号を表します。 |
| `homeAddress` | [住所](../../data-types/postal-address.md) | 人物の住所を表します。 |
| `homePhone` | [電話番号](../../data-types/phone-number.md) | 人物の自宅電話番号を表します。 |
| `mobilePhone` | [電話番号](../../data-types/phone-number.md) | 人物の携帯電話番号を表します。 |
| `personalEmail` | [メールアドレス](../../data-types/email-address.md) | 人物の電子メールアドレスを表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
