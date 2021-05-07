---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ；個人の詳細；スキーマ設計；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 個人連絡先の詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、[個人の連絡先の詳細スキーマ]フィールドグループの概要を説明します。
exl-id: a78d9aee-ecf6-45a9-b270-cdad5b800a86
translation-type: tm+mt
source-git-commit: 4755f9b7666efd8354a5f15aeed40a7da4a06efe
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 8%

---


# [!UICONTROL 個人連絡先] 詳細スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 個人連絡先] 詳細は、 [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) 分類の標準スキーマフィールドグループで、個人の連絡先情報を示します。

![](../../images/field-groups/personal-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `faxPhone` | [電話番号](../../data-types/phone-number.md) | ユーザーのFAX番号を示します。 |
| `homeAddress` | [住所](../../data-types/postal-address.md) | 人の住所を表します。 |
| `homePhone` | [電話番号](../../data-types/phone-number.md) | その人の自宅の電話番号を示します。 |
| `mobilePhone` | [電話番号](../../data-types/phone-number.md) | その人の携帯電話番号を示します。 |
| `personalEmail` | [電子メールアドレス](../../data-types/email-address.md) | ユーザーの電子メールアドレスを示します。 |

フィールドグループの詳細については、public XDM repositoryを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-personal-details.schema.json)
