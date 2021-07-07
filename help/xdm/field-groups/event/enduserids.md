---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；enduserids;end-user;end user;ids;
solution: Experience Platform
title: エンドユーザーID詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、エンドユーザーID詳細スキーマフィールドグループの概要を説明します。
exl-id: ff5b74f4-7700-4d10-821e-b50f80ea8c05
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 7%

---


# [!UICONTROL エンドユーザーID詳細スキ] ーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL エンドAdobeID ] Detailsは、クラスの標準スキーマフィールドグループで [[!DNL XDM ExperienceEvent] す](../../classes/experienceevent.md)。複数のスキーマアプリケーションをまたいで個人のID情報を記述するために使用されます。フィールドグループにはルートレベルの`endUserIDs`オブジェクトが用意されています。このオブジェクト自体には読み取り専用の`_experience`フィールドが含まれ、その値はデータの取り込み時に自動的に更新されます。

<img src="../../images/field-groups/enduserids.png" width="700" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `aacustomid` | [ID](../../data-types/identity.md) | Adobe Analytics CloudのカスタムエンドユーザーID。 |
| `aaid` | [ID](../../data-types/identity.md) | Adobe Analytics CloudのエンドユーザーID。 |
| `acid` | [ID](../../data-types/identity.md) | Adobe CampaignのエンドユーザーID。 |
| `adcloud` | [ID](../../data-types/identity.md) | Adobe Advertising CloudのエンドユーザーID。 |
| `emailid` | [ID](../../data-types/identity.md) | 電子メールアドレスID。 |
| `mcid` | [ID](../../data-types/identity.md) | Adobe Marketing Cloud ID。 |
| `phonenumberid` | [ID](../../data-types/identity.md) | 電話番号ID。 |
| `tntid` | [ID](../../data-types/identity.md) | Adobe TargetのエンドユーザーID。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.schema.json)
