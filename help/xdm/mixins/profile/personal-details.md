---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;personal details;Schema design;mixin;Mixin;
solution: Experience Platform
title: 個人の連絡先の詳細ミックスイン
topic: overview
description: このドキュメントでは、個人の連絡先の詳細ミックスインの概要を説明します。
translation-type: tm+mt
source-git-commit: f9d8021643e72e3fbb5315b54a19815dcdaaa702
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 10%

---


# [!UICONTROL 個人連絡先の詳細] Mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、 [mixin名の更新に関するドキュメントを参照してください](../name-updates.md) 。

[!UICONTROL 個人連絡先の詳細] は、 [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) クラスの標準ミックスインです。このミックスインは、個人の連絡先情報を示します。

<img src="../../images/mixins/profile-personal-details.png" width="700" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `faxPhone` | [電話番号](../../data-types/phone-number.md) | ユーザーのFAX番号を示します。 |
| `homeAddress` | [住所](../../data-types/postal-address.md) | 人の住所を表します。 |
| `homePhone` | [電話番号](../../data-types/phone-number.md) | その人の自宅の電話番号を示します。 |
| `mobilePhone` | [電話番号](../../data-types/phone-number.md) | その人の携帯電話番号を示します。 |
| `personalEmail` | [電子メールアドレス](../../data-types/email-address.md) | ユーザーの電子メールアドレスを示します。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-personal-details.schema.json)
