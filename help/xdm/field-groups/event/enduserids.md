---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；enduserids；エンドユーザー；エンドユーザー；id;
solution: Experience Platform
title: エンドユーザー ID 詳細スキーマフィールドグループ
description: エンドユーザー ID 詳細スキーマフィールドグループについて説明します。
exl-id: ff5b74f4-7700-4d10-821e-b50f80ea8c05
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 16%

---


# [!UICONTROL  エンドユーザー ID 詳細 ] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL  エンドユーザー ID 詳細 ] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループで、複数のAdobeアプリケーションをまたいで個人の ID 情報を記述するために使用されます。 フィールドグループは、ルートレベルの `endUserIDs` オブジェクトを提供します。このオブジェクトには、データが取り込まれると値が自動的に更新される読み取り専用の `_experience` フィールドが含まれています。

<img src="../../images/field-groups/enduserids.png" width="700" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `aacustomid` | [ID](../../data-types/identity.md) | Adobe Analytics Cloudのカスタムエンドユーザー ID。 |
| `aaid` | [ID](../../data-types/identity.md) | Adobe Analytics Cloudのエンドユーザー ID。 |
| `acid` | [ID](../../data-types/identity.md) | Adobe Campaignのエンドユーザー ID。 |
| `adcloud` | [ID](../../data-types/identity.md) | Adobe Advertising Cloudのエンドユーザー ID。 |
| `emailid` | [ID](../../data-types/identity.md) | メールアドレス ID。 |
| `mcid` | [ID](../../data-types/identity.md) | Adobe Marketing Cloud ID （MCID）。 MCID はExperience CloudID （ECID）になりました。 |
| `phonenumberid` | [ID](../../data-types/identity.md) | 電話番号 ID。 |
| `tntid` | [ID](../../data-types/identity.md) | Adobe Targetのエンドユーザー ID。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.schema.json)
