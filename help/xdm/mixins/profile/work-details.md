---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;Schema design;mixin;mixins;work details;profile work;
solution: Experience Platform
title: プロファイル作業の詳細ミックスイン
topic: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: e58c669b5542453b7fbf6d90deedcd2cf349c0b6
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 7%

---


# [!UICONTROL プロファイル作業の詳細] Mixin

[!UICONTROL プロファイル作業の詳細] は、 [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。 ミックスインは、勤務先住所、勤務先電子メール、勤務先電話番号、個人が属する組織など、個人に関する職業情報を取得するいくつかのフィールドを提供します。

<img src="../../images/mixins/profile-work-details.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `workAddress` | [住所](../../data-types/postal-address.md) | 個人の勤務先の住所を示します。 |
| `workEmail` | [電子メールアドレス](../../data-types/email-address.md) | ユーザーの勤務先の電子メールアドレスを示します。 |
| `workPhone` | [電話番号](../../data-types/phone-number.md) | その人の勤務先の電話番号を示します。 |
| `organizations` | 文字列（配列） | 個人が属する組織を表す自由形式の文字列の配列です。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-work-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-work-details.schema.json)