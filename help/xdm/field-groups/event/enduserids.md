---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；スキーマグループ；フィールドグループ；enduserids;end-user;end user;ids;
solution: Experience Platform
title: エンドユーザー ID 詳細スキーマフィールドグループ
description: このドキュメントでは、「 End User ID Details 」スキーマフィールドグループの概要を説明します。
exl-id: ff5b74f4-7700-4d10-821e-b50f80ea8c05
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 14%

---


# [!UICONTROL エンドユーザー ID の詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL エンドユーザー ID の詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md)：複数のアプリケーションをまたいだ個人の id 情報を表すために使用されるAdobeです。 フィールドグループは、ルートレベルを提供します `endUserIDs` オブジェクト自体に読み取り専用が含まれています。 `_experience` フィールドの値は、データの取り込み時に自動的に更新されます。

<img src="../../images/field-groups/enduserids.png" width="700" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `aacustomid` | [ID](../../data-types/identity.md) | Adobe Analytics Cloudのカスタムエンドユーザー ID。 |
| `aaid` | [ID](../../data-types/identity.md) | Adobe Analytics Cloudのエンドユーザー ID。 |
| `acid` | [ID](../../data-types/identity.md) | Adobe Campaignのエンドユーザー ID。 |
| `adcloud` | [ID](../../data-types/identity.md) | Adobe Advertising Cloudのエンドユーザー ID。 |
| `emailid` | [ID](../../data-types/identity.md) | 電子メールアドレス ID。 |
| `mcid` | [ID](../../data-types/identity.md) | Adobe Marketing Cloud ID(MCID)。 MCID は、Experience CloudID(ECID) と呼ばれます。 |
| `phonenumberid` | [ID](../../data-types/identity.md) | 電話番号 ID。 |
| `tntid` | [ID](../../data-types/identity.md) | Adobe Targetのエンドユーザー ID。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.schema.json)
