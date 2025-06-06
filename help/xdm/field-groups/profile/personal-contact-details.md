---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；個人プロファイル；フィールド；スキーマ；スキーマ；個人の詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 個人の連絡先の詳細スキーマフィールドグループ
description: 個人の連絡先の詳細スキーマフィールドグループについて説明します。
exl-id: a78d9aee-ecf6-45a9-b270-cdad5b800a86
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 16%

---


# [!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;] は、[[!DNL XDM Individual Profile]  クラスの標準スキーマフィールドグループで ](../../classes/individual-profile.md) 個人の連絡先情報を記述します。

![](../../images/field-groups/personal-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `faxPhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の FAX 番号を表します。 |
| `homeAddress` | [ 郵送先住所 ](../../data-types/postal-address.md) | 人物の居住地を表します。 |
| `homePhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の自宅電話番号を表します。 |
| `mobilePhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の携帯電話番号を表します。 |
| `personalEmail` | [ メールアドレス ](../../data-types/email-address.md) | 人物のメールアドレスを表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
