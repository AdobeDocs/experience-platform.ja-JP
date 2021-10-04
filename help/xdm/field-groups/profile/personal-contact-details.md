---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；個人の詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 個人の連絡先詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「個人の連絡先の詳細」スキーマフィールドグループの概要を説明します。
exl-id: a78d9aee-ecf6-45a9-b270-cdad5b800a86
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 10%

---


# [!UICONTROL 個人の連絡先詳細ス] キーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL 個人の連絡先] 個人の連絡先情報を説明する、 [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) 分類の標準スキーマフィールドグループの詳細。

![](../../images/field-groups/personal-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `faxPhone` | [電話番号](../../data-types/phone-number.md) | ユーザーの FAX 番号を示します。 |
| `homeAddress` | [住所](../../data-types/postal-address.md) | 個人の住所を示します。 |
| `homePhone` | [電話番号](../../data-types/phone-number.md) | 人の自宅の電話番号を表します。 |
| `mobilePhone` | [電話番号](../../data-types/phone-number.md) | ユーザーの携帯電話番号を示します。 |
| `personalEmail` | [電子メールアドレス](../../data-types/email-address.md) | ユーザーの電子メールアドレスを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
