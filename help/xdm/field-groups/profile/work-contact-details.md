---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；個人プロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；mixin;mixin；作業の詳細；プロファイル作業；
solution: Experience Platform
title: 仕事用連絡先詳細のスキーマ フィールド グループ
description: 作業連絡先の詳細スキーマフィールドグループについて説明します。
exl-id: 0133622c-e95f-4833-b2f8-3694d41751b4
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 14%

---


# [!UICONTROL &#x200B; 仕事用連絡先の詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; 仕事用連絡先の詳細 &#x200B;] は、[[!DNL XDM Individual Profile]  クラス &#x200B;](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 フィールドグループには、勤務先住所、勤務先メールアドレス、勤務先電話番号、人物が所属する組織など、個人に関する職業情報を取得する複数のフィールドが用意されています。

![](../../images/field-groups/work-contact-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `workAddress` | [&#x200B; 郵送先住所 &#x200B;](../../data-types/postal-address.md) | 人物の仕事用住所を表します。 |
| `workEmail` | [&#x200B; メールアドレス &#x200B;](../../data-types/email-address.md) | 人物の仕事用電子メールアドレスを表します。 |
| `workPhone` | [&#x200B; 電話番号 &#x200B;](../../data-types/phone-number.md) | 人物の仕事用電話番号を表します。 |
| `organizations` | 文字列（配列） | ユーザーが属する組織を表す自由形式の文字列の配列。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-work-details.schema.json)
