---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;personal details;Schema design;mixin;Mixin;
solution: Experience Platform
title: プロファイルの個人情報ミックスイン
topic: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: e58c669b5542453b7fbf6d90deedcd2cf349c0b6
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 11%

---


# [!UICONTROL プロファイルの個人情報] Mixin

[!UICONTROL プロファイルの個人の詳細] は、 [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。 ミックスインは、ルートレベルの `person` オブジェクトを提供します。このオブジェクトのサブフィールドは、個々の人に関する連絡先情報を記述します。

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
