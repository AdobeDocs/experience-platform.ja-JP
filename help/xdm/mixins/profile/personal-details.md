---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ；個人の詳細；スキーマデザイン；ミックスイン；Mixin;
solution: Experience Platform
title: 個人の連絡先の詳細Mixin
topic-legacy: overview
description: このドキュメントでは、個人の連絡先の詳細ミックスインの概要を説明します。
exl-id: a78d9aee-ecf6-45a9-b270-cdad5b800a86
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 9%

---

# [!UICONTROL 個人連絡先] の詳細Mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 個人連絡先] の詳細は、個人の連絡先情報を説明する、 [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) クラス用の標準ミックスインです。

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
